
# Plankton RDA Simulation STAC Item (Schema)

`mwj.pdes.stac-result` *v0.1*

STAC 1.0 Item cataloguing spatial field outputs of a plankton RDA simulation run, filterable by scenario, diffusion ratio and advection velocity.

[*Status*](http://www.opengis.net/def/status): Experimental

## Description

# Plankton RDA Simulation STAC Item (PROV aligned)

A **STAC 1.0 Feature** cataloguing the spatial field outputs of one plankton
RDA simulation run, using the `plankton-rda` extension namespace.

## PROV alignment (SSN §6.5)

| Property | Mapped to | Rationale |
|---|---|---|
| `plankton-rda:provenance` | `prov:wasDerivedFrom` | Links STAC Item to the SimulationRun Activity that produced it |
| `plankton-rda:observation` | `prov:used` | The ObservationCollection is a prov:Entity/Activity *consumed* by the SimulationRun. **Not** `sosa:usedProcedure` — that property's domain is `sosa:Observation`, not a plain `prov:Activity`, and the collection is not a `sosa:Procedure`. |

## Key searchable extension properties

| Property | Type | Description |
|---|---|---|
| `plankton-rda:model` | string | Always `"Scheffer1991-RDA"` |
| `plankton-rda:scenario` | enum | Attractor / pattern type |
| `plankton-rda:spatial_dimensions` | 1 or 2 | Dimensionality of the run |
| `plankton-rda:D_ratio` | number | D2/D1 — Turing instability threshold ≈ 100 |
| `plankton-rda:v_rel` | number | \|v1x − v2x\| — DIFICI threshold > 0 |
| `plankton-rda:delta` | number | Zooplankton mortality [d⁻¹] |
| `plankton-rda:boundary_condition` | enum | zero-flux / periodic |
| `plankton-rda:provenance` | URI | → PROV `SimulationRun` (`prov:wasDerivedFrom`) |
| `plankton-rda:observation` | URI | → SOSA `ObservationCollection` (`prov:used`) |

## Link relations

| `rel` | Target |
|---|---|
| `derived_from` | PROV `SimulationRun` JSON |
| `related` | SOSA `ObservationCollection` JSON |
| `software` | FiniteDifferenceMethod4PDES GitHub repo |

## Filtering via STAC API

```
GET /search?filter=plankton-rda:scenario='2D-Turing-standing-pattern'
GET /search?filter=plankton-rda:v_rel>0.005
GET /search?filter=plankton-rda:D_ratio>=100
```

## Examples

### STAC Item — 1D Turing standing pattern (run S001)
{
  "type": "Feature",
  "stac_version": "1.0.0",
  "stac_extensions": ["https://mwj.example.org/plankton-rda/v0.1/schema.json"],
  "id": "S001",
  "geometry": { "type": "LineString", "coordinates": [[0.0, 0.0], [100.0, 0.0]] },
  "bbox": [0.0, 0.0, 100.0, 0.0],
  "properties": {
    "datetime":                          "2025-01-01T00:00:00Z",
    "mwj-pdes:model":                    "Scheffer1991-RDA",
    "mwj-pdes:modelClass":               "model.lokal.o2.Plankton",
    "mwj-pdes:solver":                   "jEuler",
    "mwj-pdes:dimensions":               1,
    "mwj-pdes:scenario":                 "1D-Turing-standing-pattern",
    "mwj-pdes:provenance":               "https://mwj.example.org/pdes/procedure/plankton-euler-1d-turing",
    "mwj-pdes:observation":              "https://mwj.example.org/pdes/obs/plankton-1d-random",
    "plankton-rda:scenario":             "1D-Turing-standing-pattern",
    "plankton-rda:D1":                   1.0e-5,
    "plankton-rda:D2":                   2.0e-3,
    "plankton-rda:D_ratio":              200,
    "plankton-rda:v_rel":                0.0,
    "plankton-rda:delta":                0.175,
    "plankton-rda:boundary_condition":   "zero-flux",
    "plankton-rda:domain_length_x_cm":   100
  },
  "links": [
    {
      "href":  "https://mwj.example.org/pdes/procedure/plankton-euler-1d-turing",
      "rel":   "derived_from",
      "type":  "application/json",
      "title": "SOSA SimulationProcedure"
    },
    {
      "href":  "https://mwj.example.org/pdes/obs/plankton-1d-random",
      "rel":   "related",
      "type":  "application/json",
      "title": "SOSA initial observation"
    },
    {
      "href":  "https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES",
      "rel":   "software",
      "type":  "text/html",
      "title": "FiniteDifferenceMethod4PDES"
    }
  ],
  "assets": {
    "vti_series": {
      "href":  "data/Plankton_1.vti",
      "type":  "application/vnd.vtk+xml",
      "title": "VTK Image Data time series (Plankton_1.vti … Plankton_N.vti)",
      "roles": ["data"]
    }
  }
}


### STAC Item — 2D DIFICI travelling wave (run S003)
{
  "type": "Feature",
  "stac_version": "1.0.0",
  "stac_extensions": ["https://mwj.example.org/plankton-rda/v0.1/schema.json"],
  "id": "S003",
  "geometry": { "type": "Polygon", "coordinates": [[[0,0],[100,0],[100,100],[0,100],[0,0]]] },
  "bbox": [0.0, 0.0, 100.0, 100.0],
  "properties": {
    "datetime":                          "2025-01-01T00:00:00Z",
    "mwj-pdes:model":                    "Scheffer1991-RDA",
    "mwj-pdes:modelClass":               "model.lokal.o2.Plankton",
    "mwj-pdes:solver":                   "jEuler",
    "mwj-pdes:dimensions":               2,
    "mwj-pdes:scenario":                 "2D-DIFICI-travelling-wave",
    "mwj-pdes:provenance":               "https://mwj.example.org/pdes/procedure/plankton-euler-2d-difici",
    "mwj-pdes:observation":              "https://mwj.example.org/pdes/obs/plankton-2d-random",
    "plankton-rda:scenario":             "2D-DIFICI-travelling-wave",
    "plankton-rda:D1":                   1.0e-5,
    "plankton-rda:D2":                   1.0e-5,
    "plankton-rda:D_ratio":              1,
    "plankton-rda:v_rel":                0.01,
    "plankton-rda:delta":                0.175,
    "plankton-rda:boundary_condition":   "periodic",
    "plankton-rda:domain_length_x_cm":   100,
    "plankton-rda:domain_length_y_cm":   100
  },
  "links": [
    {
      "href":  "https://mwj.example.org/pdes/procedure/plankton-euler-2d-difici",
      "rel":   "derived_from",
      "type":  "application/json",
      "title": "SOSA SimulationProcedure"
    }
  ],
  "assets": {
    "vti_series": {
      "href":  "data/Plankton_1.vti",
      "type":  "application/vnd.vtk+xml",
      "title": "VTK Image Data time series",
      "roles": ["data"]
    }
  }
}

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: PlanktonRDASTACItem
description: 'A STAC 1.0 Item cataloguing outputs of a plankton RDA simulation run,
  with plankton-rda extension properties for scenario, diffusion ratio and advection
  velocity.

  '
type: object
required:
- type
- stac_version
- id
- geometry
- bbox
- properties
- links
- assets
properties:
  type:
    const: Feature
  stac_version:
    type: string
    const: 1.0.0
  stac_extensions:
    type: array
    items:
      type: string
  id:
    type: string
    description: Unique run identifier, e.g. 'S001'
  geometry:
    type: object
    description: 'GeoJSON geometry of the simulation domain. Point for ODE, LineString
      for 1D PDE, Polygon for 2D PDE.

      '
    required:
    - type
    - coordinates
    properties:
      type:
        type: string
        enum:
        - Point
        - LineString
        - Polygon
      coordinates: {}
  bbox:
    type: array
    minItems: 4
    items:
      type: number
  properties:
    type: object
    required:
    - datetime
    - mwj-pdes:model
    - plankton-rda:scenario
    properties:
      datetime:
        type: string
        format: date-time
        description: Simulation start time (ISO 8601)
      mwj-pdes:model:
        type: string
        const: Scheffer1991-RDA
      mwj-pdes:modelClass:
        type: string
        const: model.lokal.o2.Plankton
      mwj-pdes:solver:
        type: string
        enum:
        - jEuler
        - jRungeKutta
        - jButcher
      mwj-pdes:dimensions:
        type: integer
        enum:
        - 0
        - 1
        - 2
        description: 0 = ODE only, 1 = 1D PDE, 2 = 2D PDE
      mwj-pdes:provenance:
        type: string
        description: URI of the PlanktonSimulationProcedure
      mwj-pdes:observation:
        type: string
        description: URI of the initial ObservationCollection
      plankton-rda:scenario:
        type: string
        enum:
        - ODE-stable-node
        - ODE-stable-focus
        - ODE-limit-cycle
        - 1D-Turing-standing-pattern
        - 2D-Turing-standing-pattern
        - 1D-DIFICI-travelling-wave
        - 2D-DIFICI-travelling-wave
        x-jsonld-id: https://mwj.example.org/plankton/stac/scenario
      plankton-rda:D1:
        type: number
        description: Phytoplankton diffusivity D1 [cm^2 d^-1]
        x-jsonld-id: https://mwj.example.org/plankton/stac/D1
      plankton-rda:D2:
        type: number
        description: Zooplankton diffusivity D2 [cm^2 d^-1]
        x-jsonld-id: https://mwj.example.org/plankton/stac/D2
      plankton-rda:D_ratio:
        type: number
        description: "D2/D1 ratio \u2014 ratio > ~100 triggers Turing instability"
        x-jsonld-id: https://mwj.example.org/plankton/stac/D_ratio
      plankton-rda:v_rel:
        type: number
        description: "|v1x - v2x| [cm d^-1] \u2014 > 0 triggers DIFICI travelling
          waves"
        x-jsonld-id: https://mwj.example.org/plankton/stac/v_rel
      plankton-rda:delta:
        type: number
        description: "Zooplankton mortality rate delta [d^-1] \u2014 primary bifurcation
          parameter"
        x-jsonld-id: https://mwj.example.org/plankton/stac/delta
      plankton-rda:boundary_condition:
        type: string
        enum:
        - zero-flux
        - periodic
        - dirichlet
        x-jsonld-id: https://mwj.example.org/plankton/stac/boundary_condition
      plankton-rda:domain_length_x_cm:
        type: number
        description: Domain length in x [cm]
        x-jsonld-id: https://mwj.example.org/plankton/stac/domain_length_x_cm
      plankton-rda:domain_length_y_cm:
        type: number
        description: "Domain length in y [cm] \u2014 2D only"
        x-jsonld-id: https://mwj.example.org/plankton/stac/domain_length_y_cm
  links:
    type: array
    items:
      type: object
      required:
      - href
      - rel
      properties:
        href:
          type: string
        rel:
          type: string
        type:
          type: string
        title:
          type: string
  assets:
    type: object
    description: "STAC assets \u2014 at minimum the VTI time-series"
    properties:
      vti_series:
        type: object
        properties:
          href:
            type: string
          type:
            type: string
          title:
            type: string
          roles:
            type: array
            items:
              type: string
x-jsonld-extra-terms:
  plankton-rda:provenance:
    x-jsonld-type: '@id'
  plankton-rda:observation:
    x-jsonld-type: '@id'
x-jsonld-prefixes:
  prov: http://www.w3.org/ns/prov#
  sosa: http://www.w3.org/ns/sosa/
  dcat: http://www.w3.org/ns/dcat#
  plankton-rda: https://mwj.example.org/plankton/stac/

```

Links to the schema:

* YAML version: [schema.yaml](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/stac-result/schema.json)
* JSON version: [schema.json](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/stac-result/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "plankton-rda:provenance": {
      "@type": "@id"
    },
    "plankton-rda:observation": {
      "@type": "@id"
    },
    "prov": "http://www.w3.org/ns/prov#",
    "sosa": "http://www.w3.org/ns/sosa/",
    "dcat": "http://www.w3.org/ns/dcat#",
    "plankton-rda": "https://mwj.example.org/plankton/stac/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/stac-result/context.jsonld)

## Sources

* [STAC Specification 1.0.0](https://stacspec.org/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/MarkusWilhelmJahn/bblocks-pdes-plankton](https://github.com/MarkusWilhelmJahn/bblocks-pdes-plankton)
* Path: `_sources/stac-result`

