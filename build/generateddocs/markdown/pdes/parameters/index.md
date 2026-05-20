
# Scheffer Phytoplankton-Zooplankton RDA Parameters (Schema)

`mwj.pdes.parameters` *v0.1*

Typed parameter set for the Scheffer (1991) phytoplankton/zooplankton reaction-diffusion-advection model (Malchow, GlgbasModII WS2010, eqs. 4.64/4.65). Covers reaction parameters, diffusion coefficients and advection velocities.

[*Status*](http://www.opengis.net/def/status): Experimental

## Description

# Scheffer Phytoplankton-Zooplankton RDA Parameters

Parameter schema for the two-species **reaction-diffusion-advection** model
from Malchow, *Gleichungsbasierte Modelle II* (WS 2010/11), equations 4.64/4.65.
Original local kinetics from Scheffer (1991).

## Governing equations

```
∂X1/∂t = α·N/(HN+N)·X1 − c·X1² − γ·X1/(H1+X1)·X2 − v1·∇X1 + D1·∇²X1
∂X2/∂t = e·γ·X1/(H1+X1)·X2 − δ·X2 − F·X2²/(H2²+X2²) − v2·∇X2 + D2·∇²X2
```

- **X1** = phytoplankton density [mg.dw l⁻¹]
- **X2** = zooplankton density [mg.dw l⁻¹]
- Holling Type II functional response for zooplankton grazing
- Holling Type III for fish predation on zooplankton

## Default parameter values (Malchow 2000 / Scheffer 1991)

| Parameter | Value | Unit | Description |
|-----------|-------|------|-------------|
| α | 0.5 | d⁻¹ | Max phytoplankton growth rate |
| c | 0.05 | mg.dw⁻¹ l d⁻¹ | Intraspecific competition |
| γ | 0.4 | d⁻¹ | Max grazing rate |
| H1 | 0.6 | mg.dw l⁻¹ | Half-sat grazing (Holling II) |
| H2 | 5.0 | mg.dw l⁻¹ | Half-sat fish predation (Holling III) |
| HN | 1.0 | rel. | Half-sat nutrient uptake |
| e | 0.6 | — | Assimilation efficiency |
| **δ** | **0.175** | **d⁻¹** | **Zooplankton mortality (bifurcation param)** |
| N | 2.5 | rel. | Nutrient level (control) |
| F | 0.4 | mg.dw d⁻¹ l⁻¹ | Fish predation (control) |

## Spatial scenarios (from Figs. 4.4–4.8 of script)

| Scenario | D1 | D2 | v1x−v2x | Outcome |
|----------|----|----|---------|---------|
| Turing structures | 10⁻⁵ | 2×10⁻³ | 0 | Standing spatial patterns |
| Travelling waves (DIFICI) | 10⁻⁵ | 10⁻⁵ | 0.01 | Propagating population waves |
| ODE only | 0 | 0 | 0 | Local oscillations / fixed points |

## Implemented in

[FiniteDifferenceMethod4PDES](https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES)
— explicit finite difference solver for 1D and 2D domains with zero-flux
and periodic boundary conditions.

## Examples

### ODE only — Turing scenario reaction parameters (delta=0.175, no space)
{
  "alpha": 0.5, "c": 0.05, "gamma": 0.4,
  "H1": 0.6, "H2": 5.0, "HN": 1.0,
  "e": 0.6, "delta": 0.175, "N": 2.5, "F": 0.4,
  "D1": 0.0, "D2": 0.0,
  "v1x": 0.0, "v2x": 0.0, "v1y": 0.0, "v2y": 0.0,
  "spatial_dimensions": 1,
  "domain_length_x_cm": 100,
  "grid_points_x": 200,
  "dt": 0.01, "t_end": 700,
  "boundary_condition": "zero-flux"
}


### 1D Turing structures — D1=1e-5, D2=2e-3, zero-flux (Fig. 4.4)
{
  "alpha": 0.5, "c": 0.05, "gamma": 0.4,
  "H1": 0.6, "H2": 5.0, "HN": 1.0,
  "e": 0.6, "delta": 0.175, "N": 2.5, "F": 0.4,
  "D1": 1.0e-5, "D2": 2.0e-3,
  "v1x": 0.0, "v2x": 0.0, "v1y": 0.0, "v2y": 0.0,
  "spatial_dimensions": 1,
  "domain_length_x_cm": 100,
  "grid_points_x": 200,
  "dt": 0.1, "t_end": 1600,
  "boundary_condition": "zero-flux"
}


### 1D Travelling waves (DIFICI) — equal diffusion, differential advection (Fig. 4.5)
{
  "alpha": 0.5, "c": 0.05, "gamma": 0.4,
  "H1": 0.6, "H2": 5.0, "HN": 1.0,
  "e": 0.6, "delta": 0.175, "N": 2.5, "F": 0.4,
  "D1": 1.0e-5, "D2": 1.0e-5,
  "v1x": 0.01, "v2x": 0.0, "v1y": 0.0, "v2y": 0.0,
  "spatial_dimensions": 1,
  "domain_length_x_cm": 100,
  "grid_points_x": 200,
  "dt": 0.1, "t_end": 1600,
  "boundary_condition": "zero-flux"
}


### 2D Turing structures — 1m² domain, zero-flux all sides (Fig. 4.6)
{
  "alpha": 0.5, "c": 0.05, "gamma": 0.4,
  "H1": 0.6, "H2": 5.0, "HN": 1.0,
  "e": 0.6, "delta": 0.175, "N": 2.5, "F": 0.4,
  "D1": 1.0e-5, "D2": 2.0e-3,
  "v1x": 0.0, "v2x": 0.0, "v1y": 0.0, "v2y": 0.0,
  "spatial_dimensions": 2,
  "domain_length_x_cm": 100,
  "domain_length_y_cm": 100,
  "grid_points_x": 100,
  "grid_points_y": 100,
  "dt": 0.1, "t_end": 1600,
  "boundary_condition": "zero-flux"
}


### 2D Travelling waves — diagonal differential advection, periodic L/R (Fig. 4.8)
{
  "alpha": 0.5, "c": 0.05, "gamma": 0.4,
  "H1": 0.6, "H2": 5.0, "HN": 1.0,
  "e": 0.6, "delta": 0.175, "N": 2.5, "F": 0.4,
  "D1": 1.0e-5, "D2": 1.0e-5,
  "v1x": 0.01, "v2x": 0.0, "v1y": 0.01, "v2y": 0.0,
  "spatial_dimensions": 2,
  "domain_length_x_cm": 100,
  "domain_length_y_cm": 100,
  "grid_points_x": 100,
  "grid_points_y": 100,
  "dt": 0.1, "t_end": 1600,
  "boundary_condition": "periodic"
}

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: PlanktonRDAParameters
description: "Parameters for the Scheffer phytoplankton (X1) / zooplankton (X2) reaction-diffusion-advection
  model (Malchow GlgbasModII WS2010, eqs. 4.64/4.65):\n\n  dX1/dt = alpha * N/(HN+N)
  * X1  -  c*X1^2  -  gamma * X1/(H1+X1) * X2\n           - v1 \xB7 grad(X1)  +  D1
  \xB7 lap(X1)\n\n  dX2/dt = e*gamma * X1/(H1+X1) * X2  -  delta*X2\n           -
  F * X2^2/(H2^2+X2^2)\n           - v2 \xB7 grad(X2)  +  D2 \xB7 lap(X2)\n\nN (nutrients)
  and F (fish predation) are external control parameters. Default values from Malchow
  (2000) / Scheffer (1991): p. 4275 of script.\n"
type: object
required:
- alpha
- c
- gamma
- H1
- H2
- HN
- e
- delta
- F
- N
- D1
- D2
properties:
  alpha:
    type: number
    minimum: 0
    description: 'Maximum phytoplankton growth rate [d^-1]. Default: 0.5'
    default: 0.5
    x-jsonld-id: https://mwj.example.org/plankton/model/maxPhytoplanktonGrowthRate
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  c:
    type: number
    minimum: 0
    description: 'Phytoplankton intraspecific competition coefficient [mg.dw^-1 l
      d^-1]. Default: 0.05'
    default: 0.05
    x-jsonld-id: https://mwj.example.org/plankton/model/competitionCoefficient
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  gamma:
    type: number
    minimum: 0
    description: 'Maximum zooplankton grazing rate on phytoplankton (Holling Type
      II) [d^-1]. Default: 0.4'
    default: 0.4
    x-jsonld-id: https://mwj.example.org/plankton/model/maxGrazingRate
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  H1:
    type: number
    minimum: 0
    description: 'Half-saturation constant for zooplankton grazing [mg.dw l^-1]. Default:
      0.6'
    default: 0.6
    x-jsonld-id: https://mwj.example.org/plankton/model/halfSatGrazing
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  H2:
    type: number
    minimum: 0
    description: 'Half-saturation constant for fish predation on zooplankton (Holling
      Type III) [mg.dw l^-1]. Default: 5.0'
    default: 5.0
    x-jsonld-id: https://mwj.example.org/plankton/model/halfSatFishPredation
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  HN:
    type: number
    minimum: 0
    description: 'Half-saturation constant for nutrient-limited phytoplankton growth
      (Michaelis-Menten) [relative units]. Default: 1.0'
    default: 1.0
    x-jsonld-id: https://mwj.example.org/plankton/model/halfSatNutrient
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  e:
    type: number
    minimum: 0
    maximum: 1
    description: 'Zooplankton assimilation efficiency (dimensionless). Default: 0.6'
    default: 0.6
    x-jsonld-id: https://mwj.example.org/plankton/model/assimilationEfficiency
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  delta:
    type: number
    minimum: 0
    description: 'Zooplankton mortality and respiration rate [d^-1]. Default: 0.175.
      Primary bifurcation parameter for ODE attractor type.'
    default: 0.175
    x-jsonld-id: https://mwj.example.org/plankton/model/zooplanktonMortalityRate
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  N:
    type: number
    minimum: 0
    description: "Nutrient level \u2014 external control parameter (relative units).
      Default: 2.5"
    default: 2.5
    x-jsonld-id: https://mwj.example.org/plankton/model/nutrientLevel
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  F:
    type: number
    minimum: 0
    description: 'Fish predation rate on zooplankton [mg.dw d^-1 l^-1]. Default: 0.4'
    default: 0.4
    x-jsonld-id: https://mwj.example.org/plankton/model/fishPredationRate
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  D1:
    type: number
    minimum: 0
    description: 'Phytoplankton diffusion coefficient [cm^2 d^-1]. Turing scenario:
      D1=1e-5, D2=2e-3 (ratio 200). Travelling wave (DIFICI) scenario: D1=D2=1e-5.

      '
    default: 1.0e-05
    x-jsonld-id: https://mwj.example.org/plankton/model/phytoplanktonDiffusivity
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  D2:
    type: number
    minimum: 0
    description: 'Zooplankton diffusion coefficient [cm^2 d^-1]. Turing scenario:
      D1=1e-5, D2=2e-3. Travelling wave scenario: D1=D2=1e-5.

      '
    default: 1.0e-05
    x-jsonld-id: https://mwj.example.org/plankton/model/zooplanktonDiffusivity
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  v1x:
    type: number
    description: 'Phytoplankton advection velocity in x-direction [cm d^-1]. Default:
      0.0'
    default: 0.0
    x-jsonld-id: https://mwj.example.org/plankton/model/phytoplanktonAdvectionX
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  v2x:
    type: number
    description: 'Zooplankton advection velocity in x-direction [cm d^-1]. DIFICI
      (travelling waves) requires v1x - v2x != 0, e.g. 0.01. Default: 0.0

      '
    default: 0.0
    x-jsonld-id: https://mwj.example.org/plankton/model/zooplanktonAdvectionX
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  v1y:
    type: number
    description: 'Phytoplankton advection velocity in y-direction [cm d^-1]. Default:
      0.0'
    default: 0.0
    x-jsonld-id: https://mwj.example.org/plankton/model/phytoplanktonAdvectionY
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  v2y:
    type: number
    description: 'Zooplankton advection velocity in y-direction [cm d^-1]. Default:
      0.0'
    default: 0.0
    x-jsonld-id: https://mwj.example.org/plankton/model/zooplanktonAdvectionY
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  domain_length_x_cm:
    type: number
    minimum: 0
    description: 'Spatial domain length in x [cm]. Script examples: 100 cm (1D), 50/100/150
      cm (2D).'
  domain_length_y_cm:
    type: number
    minimum: 0
    description: Spatial domain length in y [cm]. Only for 2D runs.
  grid_points_x:
    type: integer
    minimum: 2
    description: Number of grid points in x-direction (FDM spatial discretisation).
  grid_points_y:
    type: integer
    minimum: 2
    description: Number of grid points in y-direction. Only for 2D runs.
  dt:
    type: number
    minimum: 0
    exclusiveMinimum: 0
    description: Time step [d]. Must satisfy CFL condition for explicit FDM schemes.
    x-jsonld-id: https://mwj.example.org/plankton/model/numericalTimeStep
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  t_end:
    type: number
    minimum: 0
    description: Simulation end time [d].
    x-jsonld-id: https://mwj.example.org/plankton/model/simulationEndTime
    x-jsonld-type: http://qudt.org/schema/qudt/Quantity
  boundary_condition:
    type: string
    enum:
    - zero-flux
    - periodic
    - dirichlet
    description: 'Spatial boundary condition type. zero-flux (Neumann, zero-gradient):
      used in Turing-structure runs. periodic: used on left/right boundary in travelling-wave
      run (Fig. 4.8).

      '
    x-jsonld-id: https://mwj.example.org/plankton/model/boundaryConditionType
  spatial_dimensions:
    type: integer
    enum:
    - 1
    - 2
    description: Number of spatial dimensions (1D or 2D).
    x-jsonld-id: https://mwj.example.org/plankton/model/spatialDimensions
x-jsonld-prefixes:
  plk: https://mwj.example.org/plankton/model/
  qudt: http://qudt.org/schema/qudt/

```

Links to the schema:

* YAML version: [schema.yaml](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/parameters/schema.json)
* JSON version: [schema.json](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/parameters/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "alpha": {
      "@id": "plk:maxPhytoplanktonGrowthRate",
      "@type": "qudt:Quantity"
    },
    "c": {
      "@id": "plk:competitionCoefficient",
      "@type": "qudt:Quantity"
    },
    "gamma": {
      "@id": "plk:maxGrazingRate",
      "@type": "qudt:Quantity"
    },
    "H1": {
      "@id": "plk:halfSatGrazing",
      "@type": "qudt:Quantity"
    },
    "H2": {
      "@id": "plk:halfSatFishPredation",
      "@type": "qudt:Quantity"
    },
    "HN": {
      "@id": "plk:halfSatNutrient",
      "@type": "qudt:Quantity"
    },
    "e": {
      "@id": "plk:assimilationEfficiency",
      "@type": "qudt:Quantity"
    },
    "delta": {
      "@id": "plk:zooplanktonMortalityRate",
      "@type": "qudt:Quantity"
    },
    "N": {
      "@id": "plk:nutrientLevel",
      "@type": "qudt:Quantity"
    },
    "F": {
      "@id": "plk:fishPredationRate",
      "@type": "qudt:Quantity"
    },
    "D1": {
      "@id": "plk:phytoplanktonDiffusivity",
      "@type": "qudt:Quantity"
    },
    "D2": {
      "@id": "plk:zooplanktonDiffusivity",
      "@type": "qudt:Quantity"
    },
    "v1x": {
      "@id": "plk:phytoplanktonAdvectionX",
      "@type": "qudt:Quantity"
    },
    "v2x": {
      "@id": "plk:zooplanktonAdvectionX",
      "@type": "qudt:Quantity"
    },
    "v1y": {
      "@id": "plk:phytoplanktonAdvectionY",
      "@type": "qudt:Quantity"
    },
    "v2y": {
      "@id": "plk:zooplanktonAdvectionY",
      "@type": "qudt:Quantity"
    },
    "dt": {
      "@id": "plk:numericalTimeStep",
      "@type": "qudt:Quantity"
    },
    "t_end": {
      "@id": "plk:simulationEndTime",
      "@type": "qudt:Quantity"
    },
    "boundary_condition": "plk:boundaryConditionType",
    "spatial_dimensions": "plk:spatialDimensions",
    "plk": "https://mwj.example.org/plankton/model/",
    "qudt": "http://qudt.org/schema/qudt/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/parameters/context.jsonld)

## Sources

* [Scheffer (1991) Oikos 62:271-282](https://doi.org/10.2307/3545491)
* [Malchow — Gleichungsbasierte Modelle II, Kap. 4.6 (WS 2010/11)](https://www.usf.uos.de/~malchow)
* [Simulation software — FiniteDifferenceMethod4PDES](https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/MarkusWilhelmJahn/bblocks-pdes-plankton](https://github.com/MarkusWilhelmJahn/bblocks-pdes-plankton)
* Path: `_sources/parameters`

