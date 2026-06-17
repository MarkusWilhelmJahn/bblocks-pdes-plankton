
# Plankton Simulation Procedure (SOSA) (Schema)

`mwj.pdes.simulation-run` *v0.1*

A sosa:Procedure describing how to execute the Scheffer (1991) phytoplankton/zooplankton RDA model in FiniteDifferenceMethod4PDES, specifying the model class, numerical solver, spatial domain and time discretisation.

[*Status*](http://www.opengis.net/def/status): Experimental

## Description

# Plankton Simulation Procedure (SOSA)

A **sosa:Procedure** describing how to execute the Scheffer (1991)
phytoplankton/zooplankton RDA model in
[FiniteDifferenceMethod4PDES](https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES).

## SOSA alignment

`SimulationProcedure` is a `sosa:Procedure`. It is referenced from a
`sosa:ObservationCollection` (initial conditions) via `sosa:usedProcedure`.
Each execution produces a VTI time series catalogued as a STAC Item.

```
sosa:ObservationCollection  ──sosa:usedProcedure──► PlanktonSimulationProcedure
  (initial conditions,                                (sosa:Procedure)
   mwj.pdes.observation)                                    │
                                                    sosa:implementedBy
                                              FiniteDifferenceMethod4PDES
                                                    (prov:Agent)
```

## Model equations

```
dX₁/dt = alpha · N/(HN+N) · X₁  −  c · X₁²  −  gamma · X₁/(H₁+X₁) · X₂
dX₂/dt = e · gamma · X₁/(H₁+X₁) · X₂  −  delta · X₂  −  F · X₂²/(H₂²+X₂²)
```

For PDE runs, spatial operators are added:

```
∂Xᵢ/∂t = (local kinetics)  −  vᵢ · ∇Xᵢ  +  Dᵢ · ∇²Xᵢ
```

## Scenario classification

| `scenario` | D₁ | D₂ | v₁ₓ−v₂ₓ | Pattern |
|---|---|---|---|---|
| `ODE-stable-node` | 0 | 0 | 0 | Decaying to fixed point |
| `ODE-stable-focus` | 0 | 0 | 0 | Damped oscillations |
| `ODE-limit-cycle` | 0 | 0 | 0 | Sustained oscillations |
| `1D-Turing-standing-pattern` | 10⁻⁵ | 2×10⁻³ | 0 | Stationary pattern (Fig. 4.4) |
| `2D-Turing-standing-pattern` | 10⁻⁵ | 2×10⁻³ | 0 | 2D surface (Figs. 4.6, 4.7) |
| `1D-DIFICI-travelling-wave` | 10⁻⁵ | 10⁻⁵ | 0.01 | Moving wave (Fig. 4.5) |
| `2D-DIFICI-travelling-wave` | 10⁻⁵ | 10⁻⁵ | 0.01 | 2D moving wave (Fig. 4.8) |

## Examples

### ODE run — Plankton stable focus (Euler, no space)
{
  "type": "SimulationProcedure",
  "id": "https://mwj.example.org/pdes/procedure/plankton-ode-stable-focus",
  "label": "Plankton-ODE-stable-focus",
  "modelClass": "model.lokal.o2.Plankton",
  "software": {
    "id": "https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES",
    "label": "FiniteDifferenceMethod4PDES",
    "version": "1.0.0"
  },
  "simulationDomain": "https://mwj.example.org/pdes/domain/plankton-phase-portrait",
  "numericalSolver": "jEuler",
  "dt": 0.01,
  "t_end": 700.0,
  "scenario": "ODE-stable-focus",
  "parameters": {
    "alpha": 0.5, "c": 0.05, "gamma": 0.4,
    "H1": 0.6, "H2": 5.0, "HN": 1.0,
    "e": 0.6, "delta": 0.175, "N": 2.5, "F": 0.4,
    "D1": 0.0, "D2": 0.0,
    "v1x": 0.0, "v2x": 0.0
  }
}


### PDE run — Plankton 1D Turing standing pattern (Euler + diffusion, Fig. 4.4)
{
  "type": "SimulationProcedure",
  "id": "https://mwj.example.org/pdes/procedure/plankton-euler-1d-turing",
  "label": "Plankton-Euler-1D-Turing",
  "modelClass": "model.lokal.o2.Plankton",
  "software": {
    "id": "https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES",
    "label": "FiniteDifferenceMethod4PDES",
    "version": "1.0.0"
  },
  "simulationDomain": "https://mwj.example.org/pdes/domain/plankton-1d-100cm",
  "numericalSolver": "jEuler",
  "dt": 0.1,
  "t_end": 1600.0,
  "scenario": "1D-Turing-standing-pattern",
  "parameters": {
    "alpha": 0.5, "c": 0.05, "gamma": 0.4,
    "H1": 0.6, "H2": 5.0, "HN": 1.0,
    "e": 0.6, "delta": 0.175, "N": 2.5, "F": 0.4,
    "D1": 1.0e-5, "D2": 2.0e-3,
    "v1x": 0.0, "v2x": 0.0
  },
  "spatialSetup": {
    "dimensions": 1,
    "domain_length_x": 100.0,
    "grid_points_x": 200,
    "boundary_condition": "zero-flux",
    "diffusion": true,
    "advection": false
  }
}


### PDE run — Plankton 1D DIFICI travelling wave (Euler + advection, Fig. 4.5)
{
  "type": "SimulationProcedure",
  "id": "https://mwj.example.org/pdes/procedure/plankton-euler-1d-difici",
  "label": "Plankton-Euler-1D-DIFICI",
  "modelClass": "model.lokal.o2.Plankton",
  "software": {
    "id": "https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES",
    "label": "FiniteDifferenceMethod4PDES",
    "version": "1.0.0"
  },
  "simulationDomain": "https://mwj.example.org/pdes/domain/plankton-1d-100cm",
  "numericalSolver": "jEuler",
  "dt": 0.1,
  "t_end": 1600.0,
  "scenario": "1D-DIFICI-travelling-wave",
  "parameters": {
    "alpha": 0.5, "c": 0.05, "gamma": 0.4,
    "H1": 0.6, "H2": 5.0, "HN": 1.0,
    "e": 0.6, "delta": 0.175, "N": 2.5, "F": 0.4,
    "D1": 1.0e-5, "D2": 1.0e-5,
    "v1x": 0.01, "v2x": 0.0
  },
  "spatialSetup": {
    "dimensions": 1,
    "domain_length_x": 100.0,
    "grid_points_x": 200,
    "boundary_condition": "zero-flux",
    "diffusion": true,
    "advection": true
  }
}


### PDE run — Plankton 2D Turing structures (Euler + diffusion, Fig. 4.6)
{
  "type": "SimulationProcedure",
  "id": "https://mwj.example.org/pdes/procedure/plankton-euler-2d-turing",
  "label": "Plankton-Euler-2D-Turing",
  "modelClass": "model.lokal.o2.Plankton",
  "software": {
    "id": "https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES",
    "label": "FiniteDifferenceMethod4PDES",
    "version": "1.0.0"
  },
  "simulationDomain": "https://mwj.example.org/pdes/domain/plankton-2d-100x100cm",
  "numericalSolver": "jEuler",
  "dt": 0.1,
  "t_end": 1600.0,
  "scenario": "2D-Turing-standing-pattern",
  "parameters": {
    "alpha": 0.5, "c": 0.05, "gamma": 0.4,
    "H1": 0.6, "H2": 5.0, "HN": 1.0,
    "e": 0.6, "delta": 0.175, "N": 2.5, "F": 0.4,
    "D1": 1.0e-5, "D2": 2.0e-3,
    "v1x": 0.0, "v2x": 0.0, "v1y": 0.0, "v2y": 0.0
  },
  "spatialSetup": {
    "dimensions": 2,
    "domain_length_x": 100.0,
    "domain_length_y": 100.0,
    "grid_points_x": 100,
    "grid_points_y": 100,
    "boundary_condition": "zero-flux",
    "diffusion": true,
    "advection": false
  }
}

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: PlanktonSimulationProcedure
description: 'A sosa:Procedure describing how to execute the Scheffer (1991) phytoplankton/zooplankton
  RDA model in FiniteDifferenceMethod4PDES. Referenced from a sosa:ObservationCollection
  via sosa:usedProcedure.

  '
type: object
required:
- type
- modelClass
- software
- numericalSolver
- dt
- t_end
properties:
  type:
    const: SimulationProcedure
  id:
    type: string
    description: URI identifying this procedure
    x-jsonld-id: '@id'
  label:
    type: string
    description: Human-readable label, e.g. 'Plankton-Euler-1D-Turing'
    x-jsonld-id: http://www.w3.org/2000/01/rdf-schema#label
  modelClass:
    const: model.lokal.o2.Plankton
    description: Java class implementing the Scheffer RDA model
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#modelClass
  software:
    type: object
    required:
    - id
    - label
    - version
    properties:
      id:
        type: string
        const: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES
        x-jsonld-id: '@id'
      label:
        type: string
        const: FiniteDifferenceMethod4PDES
        x-jsonld-id: http://www.w3.org/2000/01/rdf-schema#label
      version:
        type: string
    x-jsonld-id: http://www.w3.org/ns/sosa/implementedBy
  numericalSolver:
    type: string
    enum:
    - jEuler
    - jRungeKutta
    - jButcher
    description: Time-integration solver class (model.numerik.*)
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#numericalSolver
  dt:
    type: number
    minimum: 0
    exclusiveMinimum: 0
    description: Time step [d]
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#timeStep
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#double
  t_end:
    type: number
    minimum: 0
    description: Simulation end time [d]
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#endTime
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#double
  simulationDomain:
    type: string
    description: URI of the PlanktonSimulationDomain (mwj.pdes.simulation-domain)
      defining the VTK Image Data grid
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#simulationDomain
    x-jsonld-type: '@id'
  parameters:
    description: Plankton RDA parameter set
    $ref: https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/parameters/schema.yaml
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#parameters
  scenario:
    type: string
    description: Qualitative scenario label
    enum:
    - ODE-stable-node
    - ODE-stable-focus
    - ODE-limit-cycle
    - 1D-Turing-standing-pattern
    - 2D-Turing-standing-pattern
    - 1D-DIFICI-travelling-wave
    - 2D-DIFICI-travelling-wave
    x-jsonld-id: https://mwj.example.org/plankton/sim/scenario
  spatialSetup:
    type: object
    description: "PDE spatial configuration \u2014 omit for pure ODE runs"
    properties:
      dimensions:
        type: integer
        enum:
        - 1
        - 2
      domain_length_x:
        type: number
        minimum: 0
        description: Domain length in x [cm]
      domain_length_y:
        type: number
        minimum: 0
        description: "Domain length in y [cm] \u2014 2D only"
      grid_points_x:
        type: integer
        minimum: 2
      grid_points_y:
        type: integer
        minimum: 2
        description: 2D only
      boundary_condition:
        type: string
        enum:
        - zero-flux
        - periodic
        - dirichlet
      diffusion:
        type: boolean
      advection:
        type: boolean
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#spatialSetup
x-jsonld-extra-terms:
  PlanktonSimulationProcedure: http://www.w3.org/ns/sosa/Procedure
  visualisation: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#visualisation
x-jsonld-prefixes:
  sosa: http://www.w3.org/ns/sosa/
  rdfs: http://www.w3.org/2000/01/rdf-schema#
  mwj: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#
  xsd: http://www.w3.org/2001/XMLSchema#
  plksim: https://mwj.example.org/plankton/sim/

```

Links to the schema:

* YAML version: [schema.yaml](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/simulation-run/schema.json)
* JSON version: [schema.json](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/simulation-run/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "PlanktonSimulationProcedure": "sosa:Procedure",
    "visualisation": "mwj:visualisation",
    "id": "@id",
    "label": "rdfs:label",
    "modelClass": "mwj:modelClass",
    "software": "sosa:implementedBy",
    "numericalSolver": "mwj:numericalSolver",
    "dt": {
      "@id": "mwj:timeStep",
      "@type": "xsd:double"
    },
    "t_end": {
      "@id": "mwj:endTime",
      "@type": "xsd:double"
    },
    "simulationDomain": {
      "@id": "mwj:simulationDomain",
      "@type": "@id"
    },
    "parameters": {
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
        "spatial_dimensions": "plk:spatialDimensions"
      },
      "@id": "mwj:parameters"
    },
    "scenario": "plksim:scenario",
    "spatialSetup": "mwj:spatialSetup",
    "sosa": "http://www.w3.org/ns/sosa/",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "mwj": "https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "plksim": "https://mwj.example.org/plankton/sim/",
    "plk": "https://mwj.example.org/plankton/model/",
    "qudt": "http://qudt.org/schema/qudt/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/simulation-run/context.jsonld)

## Sources

* [W3C SOSA/SSN Ontology](https://www.w3.org/TR/vocab-ssn/)
* [Scheffer (1991) Oikos 62:271-282](https://doi.org/10.2307/3545491)
* [FiniteDifferenceMethod4PDES](https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/MarkusWilhelmJahn/bblocks-pdes-plankton](https://github.com/MarkusWilhelmJahn/bblocks-pdes-plankton)
* Path: `_sources/simulation-run`

