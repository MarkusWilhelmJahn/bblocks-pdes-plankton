
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

### STAC Item S001 — 1D Turing standing pattern
{
  "type": "Feature",
  "stac_version": "1.0.0",
  "stac_extensions": ["https://mwj.example.org/plankton-rda/v0.1/schema.json"],
  "id": "S001",
  "geometry": { "type": "Point", "coordinates": [9.37, 47.65] },
  "bbox": [9.35, 47.63, 9.39, 47.67],
  "properties": {
    "datetime":                          "2022-07-01T00:00:00Z",
    "start_datetime":                    "2022-07-01T00:00:00Z",
    "end_datetime":                      "2027-01-06T00:00:00Z",
    "plankton-rda:model":                "Scheffer1991-RDA",
    "plankton-rda:scenario":             "1D-Turing-standing-pattern",
    "plankton-rda:spatial_dimensions":   1,
    "plankton-rda:D1":                   1.0e-5,
    "plankton-rda:D2":                   2.0e-3,
    "plankton-rda:D_ratio":              200,
    "plankton-rda:v_rel":                0.0,
    "plankton-rda:delta":                0.175,
    "plankton-rda:boundary_condition":   "zero-flux",
    "plankton-rda:domain_length_x_cm":   100,
    "plankton-rda:t_end_d":              1600,
    "plankton-rda:numerical_scheme":     "explicit-euler",
    "plankton-rda:provenance":           "https://mwj.example.org/runs/S001",
    "plankton-rda:observation":          "https://mwj.example.org/obs/LC-2022-07-01-random"
  },
  "links": [
    {
      "href":  "https://mwj.example.org/runs/S001",
      "rel":   "derived_from",
      "type":  "application/json",
      "title": "PROV simulation run"
    },
    {
      "href":  "https://mwj.example.org/obs/LC-2022-07-01-random",
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
    "X1_field": {
      "href":  "https://mwj.example.org/data/S001/X1_field.csv",
      "type":  "text/csv",
      "title": "Phytoplankton spatial field X1(x,t)",
      "roles": ["data"]
    },
    "X2_field": {
      "href":  "https://mwj.example.org/data/S001/X2_field.csv",
      "type":  "text/csv",
      "title": "Zooplankton spatial field X2(x,t)",
      "roles": ["data"]
    },
    "PHY_profile": {
      "href":  "https://mwj.example.org/data/S001/PHY_profile_t1600.png",
      "type":  "image/png",
      "title": "Phytoplankton 1D profile at t=1600d (cf. Fig. 4.4 right)",
      "roles": ["overview"]
    },
    "wave_number": {
      "href":  "https://mwj.example.org/data/S001/wave_number_analysis.png",
      "type":  "image/png",
      "title": "G(k²) polynomial — critical wave numbers",
      "roles": ["overview"]
    }
  }
}


### STAC Item S003 — 2D DIFICI travelling wave
{
  "type": "Feature",
  "stac_version": "1.0.0",
  "stac_extensions": ["https://mwj.example.org/plankton-rda/v0.1/schema.json"],
  "id": "S003",
  "geometry": { "type": "Point", "coordinates": [9.37, 47.65] },
  "bbox": [9.35, 47.63, 9.39, 47.67],
  "properties": {
    "datetime":                          "2022-07-01T00:00:00Z",
    "start_datetime":                    "2022-07-01T00:00:00Z",
    "end_datetime":                      "2027-01-06T00:00:00Z",
    "plankton-rda:model":                "Scheffer1991-RDA",
    "plankton-rda:scenario":             "2D-DIFICI-travelling-wave",
    "plankton-rda:spatial_dimensions":   2,
    "plankton-rda:D1":                   1.0e-5,
    "plankton-rda:D2":                   1.0e-5,
    "plankton-rda:D_ratio":              1,
    "plankton-rda:v_rel":                0.01,
    "plankton-rda:delta":                0.175,
    "plankton-rda:boundary_condition":   "periodic",
    "plankton-rda:domain_length_x_cm":   100,
    "plankton-rda:domain_length_y_cm":   100,
    "plankton-rda:t_end_d":              1600,
    "plankton-rda:numerical_scheme":     "explicit-euler",
    "plankton-rda:provenance":           "https://mwj.example.org/runs/S003",
    "plankton-rda:observation":          "https://mwj.example.org/obs/LC-2022-07-01-random"
  },
  "links": [
    {
      "href":  "https://mwj.example.org/runs/S003",
      "rel":   "derived_from",
      "type":  "application/json",
      "title": "PROV simulation run"
    },
    {
      "href":  "https://mwj.example.org/obs/LC-2022-07-01-random",
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
    "PHY_t1500": {
      "href":  "https://mwj.example.org/data/S003/PHY_surface_t1500.png",
      "type":  "image/png",
      "title": "Phytoplankton 2D wave front at t=1500d (cf. Fig. 4.8)",
      "roles": ["overview"]
    },
    "PHY_t1600": {
      "href":  "https://mwj.example.org/data/S003/PHY_surface_t1600.png",
      "type":  "image/png",
      "title": "Phytoplankton 2D wave front at t=1600d",
      "roles": ["overview"]
    }
  }
}

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: PlanktonRDASTACItem
description: 'A STAC 1.0 Item cataloguing the spatial field outputs of one plankton
  RDA simulation run. Uses the ''plankton-rda'' extension namespace for model-specific
  searchable properties.

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
    const: 1.0.0
  stac_extensions:
    type: array
    items:
      type: string
  id:
    type: string
    description: Unique run identifier matching SimulationRun.id suffix
  geometry:
    type: object
    description: GeoJSON geometry of the simulation spatial domain or observation
      site
  bbox:
    type: array
    items:
      type: number
    minItems: 4
  properties:
    type: object
    required:
    - datetime
    - plankton-rda:model
    - plankton-rda:scenario
    - plankton-rda:spatial_dimensions
    properties:
      datetime:
        type: string
        format: date-time
      start_datetime:
        type: string
        format: date-time
      end_datetime:
        type: string
        format: date-time
      plankton-rda:model:
        type: string
        const: Scheffer1991-RDA
        x-jsonld-id: https://mwj.example.org/plankton/stac/model
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
      plankton-rda:spatial_dimensions:
        type: integer
        enum:
        - 1
        - 2
        x-jsonld-id: https://mwj.example.org/plankton/stac/spatial_dimensions
      plankton-rda:D1:
        type: number
        description: Phytoplankton diffusivity [cm^2 d^-1]
        x-jsonld-id: https://mwj.example.org/plankton/stac/D1
      plankton-rda:D2:
        type: number
        description: Zooplankton diffusivity [cm^2 d^-1]
        x-jsonld-id: https://mwj.example.org/plankton/stac/D2
      plankton-rda:D_ratio:
        type: number
        description: "D2/D1 ratio \u2014 >~100 triggers Turing instability"
        x-jsonld-id: https://mwj.example.org/plankton/stac/D_ratio
      plankton-rda:v_rel:
        type: number
        description: "Differential advection speed |v1x - v2x| [cm d^-1] \u2014 >0
          triggers DIFICI"
        x-jsonld-id: https://mwj.example.org/plankton/stac/v_rel
      plankton-rda:delta:
        type: number
        description: "Zooplankton mortality rate [d^-1] \u2014 bifurcation parameter"
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
        x-jsonld-id: https://mwj.example.org/plankton/stac/domain_length_x_cm
      plankton-rda:domain_length_y_cm:
        type: number
        x-jsonld-id: https://mwj.example.org/plankton/stac/domain_length_y_cm
      plankton-rda:t_end_d:
        type: number
        description: Simulation end time [d]
        x-jsonld-id: https://mwj.example.org/plankton/stac/t_end_d
      plankton-rda:numerical_scheme:
        type: string
        x-jsonld-id: https://mwj.example.org/plankton/stac/numerical_scheme
      plankton-rda:provenance:
        type: string
        description: URI of the PROV SimulationRun Activity
        x-jsonld-type: '@id'
        x-jsonld-id: https://mwj.example.org/plankton/stac/provenance
      plankton-rda:observation:
        type: string
        description: URI of the SOSA ObservationCollection used as IC
        x-jsonld-type: '@id'
        x-jsonld-id: https://mwj.example.org/plankton/stac/observation
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
          description: "'derived_from' \u2192 PROV SimulationRun; 'related'      \u2192
            SOSA ObservationCollection; 'software'     \u2192 FiniteDifferenceMethod4PDES
            repo.\n"
        type:
          type: string
        title:
          type: string
  assets:
    type: object
    description: Named output files
    additionalProperties:
      type: object
      required:
      - href
      - type
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
            enum:
            - data
            - overview
            - thumbnail
            - metadata
x-jsonld-prefixes:
  prov: http://www.w3.org/ns/prov#
  sosa: http://www.w3.org/ns/sosa/
  dcat: http://www.w3.org/ns/dcat#
  plankton-rda: https://mwj.example.org/plankton/stac/

```

Links to the schema:

* YAML version: [schema.yaml](https://markuswilhelmjahn.github.io/bblocks-pdes/build/annotated/pdes/stac-result/schema.json)
* JSON version: [schema.json](https://markuswilhelmjahn.github.io/bblocks-pdes/build/annotated/pdes/stac-result/schema.yaml)


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
[context.jsonld](https://markuswilhelmjahn.github.io/bblocks-pdes/build/annotated/pdes/stac-result/context.jsonld)

## Sources

* [STAC Specification 1.0.0](https://stacspec.org/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/MarkusWilhelmJahn/bblocks-pdes](https://github.com/MarkusWilhelmJahn/bblocks-pdes)
* Path: `_sources/stac-result`

