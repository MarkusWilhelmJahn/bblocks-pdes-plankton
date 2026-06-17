
# Plankton RDA Simulation Run (PROV) (Schema)

`mwj.pdes.simulation-run` *v0.1*

PROV-O Activity capturing a single FDM simulation of the Scheffer phytoplankton/zooplankton RDA model, linking SOSA initial observations and parameters to output spatial fields and time series.

[*Status*](http://www.opengis.net/def/status): Experimental

## Description

# Plankton RDA Simulation Run (PROV — SOSA aligned)

A **`prov:Activity`** capturing one FDM execution of the Scheffer (1991)
phytoplankton/zooplankton RDA model via
[FiniteDifferenceMethod4PDES](https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES).

## PROV alignment (SSN §6.5)

The SOSA PROV alignment defines `sosa:Observation ⊑ prov:Activity`. The
`SimulationRun` is a **separate** `prov:Activity` that **consumes** the
`ObservationCollection` (itself a collection of `prov:Activity` instances)
via `prov:used` — not via `sosa:usedProcedure`, because:

- `sosa:usedProcedure` has domain `sosa:Observation | sosa:Actuation | sosa:Sampling`
- The `SimulationRun` is none of those — it is a plain `prov:Activity`
- The `ObservationCollection` is not a `sosa:Procedure` — it is a collection of Activities

| Property | Correct mapping | Notes |
|---|---|---|
| `used.observation` | `prov:used` | ObservationCollection is a prov:Entity/Activity consumed by the run |
| `used.parametersId` | `prov:used` | Parameter set is a prov:Entity consumed by the run |
| `wasAssociatedWith` | `prov:wasAssociatedWith` | FiniteDifferenceMethod4PDES is the prov:Agent |
| `generated[*]` | `prov:wasGeneratedBy` (reverse) | Output files are prov:Entity instances |

## Lineage graph

```
sosa:ObservationCollection  ──prov:used──► SimulationRun (prov:Activity)
  (members: 4×sosa:Observation,             │
   each ⊑ prov:Activity)                    │ wasAssociatedWith
                                    FiniteDifferenceMethod4PDES (prov:Agent)
Parameter set (prov:Entity) ──prov:used──►  │
                                     prov:wasGeneratedBy
                              ┌─────────────▼──────────────────────┐
                              │ X1_field.csv   (prov:Entity)        │
                              │ X2_field.csv   (prov:Entity)        │
                              │ PHY_surface.png (prov:Entity)       │
                              └────────────────────────────────────-┘
                                             │ dcat:landingPage
                                       STAC Item
```

## Scenario classification

| `scenario` | D1 | D2 | v1x−v2x | Output |
|---|---|---|---|---|
| `ODE-*` | 0 | 0 | 0 | Time series only |
| `1D-Turing-standing-pattern` | 10⁻⁵ | 2×10⁻³ | 0 | Stationary spatial pattern (Fig. 4.4) |
| `2D-Turing-standing-pattern` | 10⁻⁵ | 2×10⁻³ | 0 | 2D surface (Figs. 4.6, 4.7) |
| `1D-DIFICI-travelling-wave` | 10⁻⁵ | 10⁻⁵ | 0.01 | Moving wave (Fig. 4.5) |
| `2D-DIFICI-travelling-wave` | 10⁻⁵ | 10⁻⁵ | 0.01 | 2D moving wave (Fig. 4.8) |

## Examples

### Run S001 — 1D Turing standing pattern (D1=1e-5, D2=2e-3)
{
  "type": "SimulationRun",
  "id":    "https://mwj.example.org/runs/S001",
  "label": "1D Turing structures — D1=1e-5, D2=2e-3, zero-flux",
  "startedAtTime": "2025-04-01T09:00:00Z",
  "endedAtTime":   "2025-04-01T09:12:34Z",
  "wasAssociatedWith": {
    "id":        "https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES",
    "label":     "FiniteDifferenceMethod4PDES",
    "version":   "main@abc1234",
    "localPath": "/Users/markusjahn/Documents/GitHub/FiniteDifferenceMethod4PDES"
  },
  "numericalScheme":       "explicit-euler",
  "spatialDiscretisation": "central-difference",
  "scenario": "1D-Turing-standing-pattern",
  "used": {
    "observation":  "https://mwj.example.org/obs/LC-2022-07-01-random",
    "parametersId": "https://mwj.example.org/params/turing-1D-default"
  },
  "parameters": {
    "alpha": 0.5, "c": 0.05, "gamma": 0.4,
    "H1": 0.6, "H2": 5.0, "HN": 1.0,
    "e": 0.6, "delta": 0.175, "N": 2.5, "F": 0.4,
    "D1": 1.0e-5, "D2": 2.0e-3,
    "v1x": 0.0, "v2x": 0.0, "v1y": 0.0, "v2y": 0.0,
    "dt": 0.1, "t_end": 1600,
    "domain_length_x_cm": 100, "grid_points_x": 200,
    "boundary_condition": "zero-flux", "spatial_dimensions": 1
  },
  "generated": [
    {
      "id":     "results/S001/X1_field.csv",
      "type":   "spatial-field-csv",
      "format": "text/csv",
      "description": "Phytoplankton density X1(x,t) — columns: x[cm], t[d], X1[mg.dw/l]"
    },
    {
      "id":     "results/S001/X2_field.csv",
      "type":   "spatial-field-csv",
      "format": "text/csv",
      "description": "Zooplankton density X2(x,t)"
    },
    {
      "id":     "results/S001/PHY_profile_t1600.png",
      "type":   "phytoplankton-surface-png",
      "format": "image/png",
      "description": "Phytoplankton spatial profile at t=1600d (cf. Fig. 4.4 right)"
    },
    {
      "id":     "results/S001/wave_number_analysis.png",
      "type":   "wave-number-plot-png",
      "format": "image/png",
      "description": "G(k^2) polynomial (eq. 4.61) showing critical wave numbers"
    }
  ],
  "stacItem": "https://mwj.example.org/stac/items/S001"
}


### Run S002 — 2D Turing structures, 1m² domain (Fig. 4.6 centre)
{
  "type": "SimulationRun",
  "id":    "https://mwj.example.org/runs/S002",
  "label": "2D Turing structures — 100x100 cm, random IC",
  "startedAtTime": "2025-04-01T10:00:00Z",
  "endedAtTime":   "2025-04-01T11:47:22Z",
  "wasAssociatedWith": {
    "id":        "https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES",
    "label":     "FiniteDifferenceMethod4PDES",
    "version":   "main@abc1234",
    "localPath": "/Users/markusjahn/Documents/GitHub/FiniteDifferenceMethod4PDES"
  },
  "numericalScheme":       "explicit-euler",
  "spatialDiscretisation": "central-difference",
  "scenario": "2D-Turing-standing-pattern",
  "used": {
    "observation":  "https://mwj.example.org/obs/LC-2022-07-01-random",
    "parametersId": "https://mwj.example.org/params/turing-2D-100x100"
  },
  "parameters": {
    "alpha": 0.5, "c": 0.05, "gamma": 0.4,
    "H1": 0.6, "H2": 5.0, "HN": 1.0,
    "e": 0.6, "delta": 0.175, "N": 2.5, "F": 0.4,
    "D1": 1.0e-5, "D2": 2.0e-3,
    "v1x": 0.0, "v2x": 0.0, "v1y": 0.0, "v2y": 0.0,
    "dt": 0.1, "t_end": 1600,
    "domain_length_x_cm": 100, "domain_length_y_cm": 100,
    "grid_points_x": 100, "grid_points_y": 100,
    "boundary_condition": "zero-flux", "spatial_dimensions": 2
  },
  "generated": [
    {
      "id":     "results/S002/PHY_surface_t1600.png",
      "type":   "phytoplankton-surface-png",
      "format": "image/png",
      "description": "2D phytoplankton density surface at t=1600d"
    },
    {
      "id":     "results/S002/ZOO_surface_t1600.png",
      "type":   "zooplankton-surface-png",
      "format": "image/png"
    }
  ],
  "stacItem": "https://mwj.example.org/stac/items/S002"
}


### Run S003 — 2D DIFICI travelling wave, diagonal advection (Fig. 4.8)
{
  "type": "SimulationRun",
  "id":    "https://mwj.example.org/runs/S003",
  "label": "2D DIFICI travelling wave — v1x=v1y=0.01, periodic L/R",
  "startedAtTime": "2025-04-01T13:00:00Z",
  "endedAtTime":   "2025-04-01T14:22:10Z",
  "wasAssociatedWith": {
    "id":        "https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES",
    "label":     "FiniteDifferenceMethod4PDES",
    "version":   "main@abc1234",
    "localPath": "/Users/markusjahn/Documents/GitHub/FiniteDifferenceMethod4PDES"
  },
  "numericalScheme":       "explicit-euler",
  "spatialDiscretisation": "upwind",
  "scenario": "2D-DIFICI-travelling-wave",
  "used": {
    "observation":  "https://mwj.example.org/obs/LC-2022-07-01-random",
    "parametersId": "https://mwj.example.org/params/difici-2D-diagonal"
  },
  "parameters": {
    "alpha": 0.5, "c": 0.05, "gamma": 0.4,
    "H1": 0.6, "H2": 5.0, "HN": 1.0,
    "e": 0.6, "delta": 0.175, "N": 2.5, "F": 0.4,
    "D1": 1.0e-5, "D2": 1.0e-5,
    "v1x": 0.01, "v2x": 0.0, "v1y": 0.01, "v2y": 0.0,
    "dt": 0.1, "t_end": 1600,
    "domain_length_x_cm": 100, "domain_length_y_cm": 100,
    "grid_points_x": 100, "grid_points_y": 100,
    "boundary_condition": "periodic", "spatial_dimensions": 2
  },
  "generated": [
    {
      "id":     "results/S003/PHY_surface_t1500.png",
      "type":   "phytoplankton-surface-png",
      "format": "image/png",
      "description": "Travelling wave front at t=1500d (cf. Fig. 4.8)"
    },
    {
      "id":     "results/S003/PHY_surface_t1600.png",
      "type":   "phytoplankton-surface-png",
      "format": "image/png",
      "description": "Travelling wave front at t=1600d"
    }
  ],
  "stacItem": "https://mwj.example.org/stac/items/S003"
}

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: PlanktonSimulationRun
description: 'A PROV Activity representing one FDM execution of the Scheffer phytoplankton/zooplankton
  RDA model using FiniteDifferenceMethod4PDES.

  '
type: object
required:
- type
- startedAtTime
- endedAtTime
- wasAssociatedWith
- used
- parameters
- generated
properties:
  type:
    const: SimulationRun
  id:
    type: string
    description: URI identifying this run
    x-jsonld-id: '@id'
  label:
    type: string
    description: Human-readable label, e.g. 'Turing-1D-D2_2e-3'
    x-jsonld-id: http://www.w3.org/2000/01/rdf-schema#label
  startedAtTime:
    type: string
    format: date-time
    x-jsonld-id: http://www.w3.org/ns/prov#startedAtTime
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#dateTime
  endedAtTime:
    type: string
    format: date-time
    x-jsonld-id: http://www.w3.org/ns/prov#endedAtTime
    x-jsonld-type: http://www.w3.org/2001/XMLSchema#dateTime
  wasAssociatedWith:
    type: object
    description: Software agent that executed the simulation (prov:wasAssociatedWith)
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
        description: Git tag or commit SHA
      localPath:
        type: string
        const: /Users/markusjahn/Documents/GitHub/FiniteDifferenceMethod4PDES
    x-jsonld-id: http://www.w3.org/ns/prov#wasAssociatedWith
  numericalScheme:
    type: string
    description: FDM time-integration scheme
    enum:
    - explicit-euler
    - implicit-euler
    - crank-nicolson
    - runge-kutta-4
    x-jsonld-id: https://mwj.example.org/plankton/sim/numericalScheme
  spatialDiscretisation:
    type: string
    description: FDM spatial discretisation scheme
    enum:
    - central-difference
    - upwind
    - lax-wendroff
    x-jsonld-id: https://mwj.example.org/plankton/sim/spatialDiscretisation
  used:
    type: object
    description: Input resources consumed (prov:used)
    required:
    - observation
    properties:
      observation:
        type: string
        description: URI of the SOSA ObservationCollection (initial conditions)
      parametersId:
        type: string
        description: URI of the parameter set entity
    x-jsonld-id: http://www.w3.org/ns/prov#used
  parameters:
    description: "Exact parameter values used \u2014 inline copy for full reproducibility"
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
      c:
        type: number
      gamma:
        type: number
      H1:
        type: number
      H2:
        type: number
      HN:
        type: number
      e:
        type: number
      delta:
        type: number
      F:
        type: number
      N:
        type: number
      D1:
        type: number
      D2:
        type: number
      v1x:
        type: number
      v2x:
        type: number
      v1y:
        type: number
      v2y:
        type: number
      dt:
        type: number
      t_end:
        type: number
      domain_length_x_cm:
        type: number
      domain_length_y_cm:
        type: number
      grid_points_x:
        type: integer
      grid_points_y:
        type: integer
      boundary_condition:
        type: string
      spatial_dimensions:
        type: integer
    x-jsonld-id: http://www.w3.org/ns/prov#used
  generated:
    type: array
    minItems: 1
    description: Output assets generated by this run (prov:wasGeneratedBy)
    items:
      type: object
      required:
      - id
      - type
      - format
      properties:
        id:
          type: string
          description: Relative path or URI to the output file
          x-jsonld-id: '@id'
        type:
          type: string
          enum:
          - timeseries-csv
          - spatial-field-csv
          - spatial-field-netcdf
          - phytoplankton-surface-png
          - zooplankton-surface-png
          - phase-portrait-png
          - wave-number-plot-png
          - bifurcation-diagram-png
        format:
          type: string
        description:
          type: string
    x-jsonld-reverse: prov:wasGeneratedBy
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
  stacItem:
    type: string
    description: URI of the STAC Item cataloguing this run's outputs
    x-jsonld-id: http://www.w3.org/ns/dcat#landingPage
x-jsonld-extra-terms:
  SimulationRun:
    x-jsonld-id: http://www.w3.org/ns/prov#Activity
x-jsonld-prefixes:
  prov: http://www.w3.org/ns/prov#
  rdfs: http://www.w3.org/2000/01/rdf-schema#
  xsd: http://www.w3.org/2001/XMLSchema#
  plksim: https://mwj.example.org/plankton/sim/
  dcat: http://www.w3.org/ns/dcat#
  sosa: http://www.w3.org/ns/sosa/
  ssn: http://www.w3.org/ns/ssn/

```

Links to the schema:

* YAML version: [schema.yaml](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/simulation-run/schema.json)
* JSON version: [schema.json](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/simulation-run/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "SimulationRun": "prov:Activity",
    "id": "@id",
    "label": "rdfs:label",
    "startedAtTime": {
      "@id": "prov:startedAtTime",
      "@type": "xsd:dateTime"
    },
    "endedAtTime": {
      "@id": "prov:endedAtTime",
      "@type": "xsd:dateTime"
    },
    "wasAssociatedWith": "prov:wasAssociatedWith",
    "numericalScheme": "plksim:numericalScheme",
    "spatialDiscretisation": "plksim:spatialDiscretisation",
    "used": "prov:used",
    "parameters": "prov:used",
    "generated": {
      "@reverse": "prov:wasGeneratedBy"
    },
    "scenario": "plksim:scenario",
    "stacItem": "dcat:landingPage",
    "prov": "http://www.w3.org/ns/prov#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "plksim": "https://mwj.example.org/plankton/sim/",
    "dcat": "http://www.w3.org/ns/dcat#",
    "sosa": "http://www.w3.org/ns/sosa/",
    "ssn": "http://www.w3.org/ns/ssn/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/simulation-run/context.jsonld)

## Sources

* [W3C PROV-O](https://www.w3.org/TR/prov-o/)
* [Simulation software — FiniteDifferenceMethod4PDES](https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/MarkusWilhelmJahn/bblocks-pdes-plankton](https://github.com/MarkusWilhelmJahn/bblocks-pdes-plankton)
* Path: `_sources/simulation-run`

