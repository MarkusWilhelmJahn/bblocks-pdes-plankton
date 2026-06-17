
# Plankton Initial Observation (SOSA) (Schema)

`mwj.pdes.observation` *v0.1*

SOSA ObservationCollection providing initial phytoplankton density X1(x,0) and zooplankton density X2(x,0) for a plankton RDA simulation run. The featureOfInterest references the SimulationDomain (VTK Image Data grid).

[*Status*](http://www.opengis.net/def/status): Experimental

## Description

# Plankton Initial Observation (SOSA — PROV aligned)

A **SOSA `ObservationCollection`** providing the initial state of a water body
or mesocosm, aligned to the SSN PROV module (W3C SSN §6.5).

## PROV alignment

| SOSA concept | PROV equivalent | Role in this block |
|---|---|---|
| `ObservationCollection` | container of `prov:Activity` instances | the collection itself |
| each member `Observation` | `prov:Activity` | one measured variable |
| `madeBySensor` (CTD sensor) | `prov:wasAssociatedWith` → `prov:Agent` | the field sensor |
| `featureOfInterest` (lake) | `prov:Entity` | the thing being measured |
| each `hasResult` | generates a `prov:Entity` | the numeric measurement |
| `usedProcedure` | `prov:used` | measurement protocol (optional) |

## Structure

Each of the four observable properties (X1, X2, N, F) is represented as a
separate `sosa:Observation` member of the collection, with its own
`observedProperty` URI, `hasResult` (value + unit), and `resultTime`.

```
ObservationCollection
  madeBySensor → ctd-003          (sosa:Sensor ⊑ prov:Agent)
  featureOfInterest → Lake        (sosa:FeatureOfInterest ⊑ prov:Entity)
  hasMember →
    Observation₁  observedProperty: phytoplanktonDensity
                  hasResult: { value: 2.15, unit: mg.dw/l }   (prov:Entity)
    Observation₂  observedProperty: zooplanktonDensity
                  hasResult: { value: 2.92, unit: mg.dw/l }
    Observation₃  observedProperty: nutrientLevel
                  hasResult: { value: 2.5,  unit: relative }
    Observation₄  observedProperty: fishPredationPressure
                  hasResult: { value: 0.4,  unit: mg.dw/d/l }
```

## Linking to the simulation

The `resultTime` of the first member Observation becomes **t = 0** of the
linked `SimulationRun`, which references this collection via `prov:used`.

## Examples

### Plankton 1D domain — initial observation from Plankton_1.vti
{
  "type": "ObservationCollection",
  "featureOfInterest": {
    "type": "Feature",
    "geometry": {
      "type": "LineString",
      "coordinates": [[0.0, 0.0], [100.0, 0.0]]
    },
    "properties": {
      "name": "Plankton 1D spatial domain",
      "simulationDomain": "https://mwj.example.org/pdes/domain/plankton-1d-100cm",
      "initVtiFile": "data/Plankton_1.vti",
      "crs": "model-units",
      "domainType": "spatial-pde",
      "gridDimensions": { "nx": 200, "ny": 1, "nz": 1 },
      "gridSpacing":    { "dx": 0.5, "dy": 1.0, "dz": 1.0 }
    }
  },
  "phenomenonTime": "2025-01-01T00:00:00Z",
  "madeBySensor": "https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES",
  "usedProcedure": "https://mwj.example.org/pdes/procedure/plankton-euler-1d-turing",
  "hasMember": [
    {
      "observedProperty": "https://mwj.example.org/pdes/obs/plankton/phytoplankton",
      "hasResult": { "value": 3.116, "unit": "mg.dw/l" },
      "resultTime": "2025-01-01T00:00:00Z"
    },
    {
      "observedProperty": "https://mwj.example.org/pdes/obs/plankton/zooplankton",
      "hasResult": { "value": 1.870, "unit": "mg.dw/l" },
      "resultTime": "2025-01-01T00:00:00Z"
    }
  ],
  "perturbation": {
    "amplitude": 0.01,
    "type": "random"
  }
}


### Plankton ODE phase-portrait — initial observation (no spatial domain)
{
  "type": "ObservationCollection",
  "featureOfInterest": {
    "type": "Feature",
    "geometry": {
      "type": "Point",
      "coordinates": [0.0, 0.0]
    },
    "properties": {
      "name": "Plankton ODE phase portrait",
      "simulationDomain": "https://mwj.example.org/pdes/domain/plankton-phase-portrait",
      "crs": "model-units",
      "domainType": "phase-portrait-ode"
    }
  },
  "phenomenonTime": "2025-01-01T00:00:00Z",
  "madeBySensor": "https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES",
  "usedProcedure": "https://mwj.example.org/pdes/procedure/plankton-ode-stable-focus",
  "hasMember": [
    {
      "observedProperty": "https://mwj.example.org/pdes/obs/plankton/phytoplankton",
      "hasResult": { "value": 3.116, "unit": "mg.dw/l" },
      "resultTime": "2025-01-01T00:00:00Z"
    },
    {
      "observedProperty": "https://mwj.example.org/pdes/obs/plankton/zooplankton",
      "hasResult": { "value": 1.870, "unit": "mg.dw/l" },
      "resultTime": "2025-01-01T00:00:00Z"
    }
  ]
}

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: PlanktonInitialObservation
description: 'A SOSA ObservationCollection providing initial conditions for a plankton
  RDA simulation run. Referenced by the SimulationProcedure via sosa:usedProcedure.

  '
type: object
required:
- type
- featureOfInterest
- phenomenonTime
- madeBySensor
- hasMember
properties:
  type:
    const: ObservationCollection
  featureOfInterest:
    type: object
    description: The sampled water body or simulation spatial domain
    required:
    - type
    - geometry
    properties:
      type:
        const: Feature
      geometry:
        type: object
        description: 'GeoJSON geometry of the domain. LineString for 1D, Polygon for
          2D, Point for ODE.

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
      properties:
        type: object
        properties:
          name:
            type: string
          crs:
            type: string
            default: model-units
          domainType:
            type: string
            enum:
            - spatial-pde
            - phase-portrait-ode
          gridDimensions:
            type: object
            properties:
              nx:
                type: integer
              ny:
                type: integer
              nz:
                type: integer
          gridSpacing:
            type: object
            properties:
              dx:
                type: number
              dy:
                type: number
              dz:
                type: number
          initVtiFile:
            type: string
            description: Path or URI to the initial VTI file (step = 1)
    x-jsonld-id: http://www.w3.org/ns/sosa/hasFeatureOfInterest
  phenomenonTime:
    type: string
    description: "ISO 8601 time instant or interval \u2014 t = 0 of the simulation"
    x-jsonld-id: http://www.w3.org/ns/sosa/phenomenonTime
  madeBySensor:
    type: string
    description: URI or name of the sensor / instrument
    x-jsonld-id: http://www.w3.org/ns/sosa/madeBySensor
  usedProcedure:
    type: string
    description: URI of the PlanktonSimulationProcedure (sosa:usedProcedure)
    x-jsonld-id: http://www.w3.org/ns/sosa/usedProcedure
    x-jsonld-type: '@id'
  hasMember:
    type: array
    minItems: 2
    description: 'One sosa:Observation per state variable. Minimum: phytoplankton
      (X1) and zooplankton (X2).

      '
    items:
      type: object
      required:
      - observedProperty
      - hasResult
      - resultTime
      properties:
        observedProperty:
          type: string
          description: "URI of the sosa:ObservableProperty. Use:\n  https://mwj.example.org/pdes/obs/plankton/phytoplankton\n
            \ https://mwj.example.org/pdes/obs/plankton/zooplankton\n  https://mwj.example.org/pdes/obs/plankton/nutrientLevel\n
            \ https://mwj.example.org/pdes/obs/plankton/fishPredationPressure\n"
          x-jsonld-id: http://www.w3.org/ns/sosa/observedProperty
          x-jsonld-type: '@id'
        hasResult:
          type: object
          required:
          - value
          properties:
            value:
              type: number
              minimum: 0
              x-jsonld-id: http://www.w3.org/ns/sosa/hasSimpleResult
            unit:
              type: string
              description: '''mg.dw/l'' for X1, X2; ''relative'' for N; ''mg.dw/d/l''
                for F'
              x-jsonld-id: http://qudt.org/schema/qudt/unit
          x-jsonld-id: http://www.w3.org/ns/sosa/hasResult
        resultTime:
          type: string
          format: date-time
          x-jsonld-id: http://www.w3.org/ns/sosa/resultTime
          x-jsonld-type: http://www.w3.org/2001/XMLSchema#dateTime
    x-jsonld-id: http://www.w3.org/ns/sosa/hasMember
  perturbation:
    type: object
    description: Optional spatial perturbation to seed pattern formation
    properties:
      amplitude:
        type: number
        description: Fraction of steady-state value
      type:
        type: string
        enum:
        - random
        - symmetric
        - asymmetric
        - none
    x-jsonld-id: https://mwj.example.org/plankton/sim/perturbation
x-jsonld-extra-terms:
  ObservationCollection: http://www.w3.org/ns/sosa/ObservationCollection
  perturbation_amplitude: https://mwj.example.org/plankton/sim/perturbationAmplitude
  perturbation_type: https://mwj.example.org/plankton/sim/perturbationType
  ObservableProperties: {}
x-jsonld-prefixes:
  sosa: http://www.w3.org/ns/sosa/
  xsd: http://www.w3.org/2001/XMLSchema#
  ssn: http://www.w3.org/ns/ssn/
  prov: http://www.w3.org/ns/prov#
  plkobs: https://mwj.example.org/plankton/obs/

```

Links to the schema:

* YAML version: [schema.yaml](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/observation/schema.json)
* JSON version: [schema.json](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/observation/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "ObservationCollection": "sosa:ObservationCollection",
    "perturbation_amplitude": "https://mwj.example.org/plankton/sim/perturbationAmplitude",
    "perturbation_type": "https://mwj.example.org/plankton/sim/perturbationType",
    "featureOfInterest": "sosa:hasFeatureOfInterest",
    "phenomenonTime": "sosa:phenomenonTime",
    "madeBySensor": "sosa:madeBySensor",
    "usedProcedure": {
      "@id": "sosa:usedProcedure",
      "@type": "@id"
    },
    "hasMember": {
      "@context": {
        "observedProperty": {
          "@id": "sosa:observedProperty",
          "@type": "@id"
        },
        "hasResult": {
          "@context": {
            "value": "sosa:hasSimpleResult",
            "unit": "http://qudt.org/schema/qudt/unit"
          },
          "@id": "sosa:hasResult"
        },
        "resultTime": {
          "@id": "sosa:resultTime",
          "@type": "xsd:dateTime"
        }
      },
      "@id": "sosa:hasMember"
    },
    "perturbation": "https://mwj.example.org/plankton/sim/perturbation",
    "sosa": "http://www.w3.org/ns/sosa/",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "ssn": "http://www.w3.org/ns/ssn/",
    "prov": "http://www.w3.org/ns/prov#",
    "plkobs": "https://mwj.example.org/plankton/obs/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/observation/context.jsonld)

## Sources

* [W3C SOSA/SSN Ontology](https://www.w3.org/TR/vocab-ssn/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/MarkusWilhelmJahn/bblocks-pdes-plankton](https://github.com/MarkusWilhelmJahn/bblocks-pdes-plankton)
* Path: `_sources/observation`

