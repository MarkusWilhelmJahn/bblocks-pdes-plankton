
# Plankton Initial Observation (SOSA) (Schema)

`mwj.pdes.observation` *v0.1*

SOSA ObservationCollection providing initial phytoplankton density X1(x,0), zooplankton density X2(x,0), nutrient level N and fish predation rate F for a RDA simulation run.

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

### Lake Constance mesocosm — four individual Observations, random IC perturbation
{
  "type": "ObservationCollection",
  "featureOfInterest": {
    "type": "Feature",
    "geometry": { "type": "Point", "coordinates": [9.37, 47.65, -5.0] },
    "properties": {
      "name": "Mesocosm 3 — Lake Constance",
      "depth_m": 5,
      "water_body_type": "lake"
    }
  },
  "phenomenonTime": "2022-06-28/2022-07-01",
  "madeBySensor": "https://mwj.example.org/sensor/ctd-003",
  "hasMember": [
    {
      "observedProperty": "https://mwj.example.org/plankton/obs/phytoplanktonDensity",
      "hasResult": { "value": 2.15, "unit": "mg.dw/l" },
      "resultTime": "2022-07-01T08:00:00Z"
    },
    {
      "observedProperty": "https://mwj.example.org/plankton/obs/zooplanktonDensity",
      "hasResult": { "value": 2.92, "unit": "mg.dw/l" },
      "resultTime": "2022-07-01T08:00:00Z"
    },
    {
      "observedProperty": "https://mwj.example.org/plankton/obs/nutrientLevel",
      "hasResult": { "value": 2.5, "unit": "relative" },
      "resultTime": "2022-07-01T08:00:00Z"
    },
    {
      "observedProperty": "https://mwj.example.org/plankton/obs/fishPredationPressure",
      "hasResult": { "value": 0.4, "unit": "mg.dw/d/l" },
      "resultTime": "2022-07-01T08:00:00Z"
    }
  ],
  "perturbation": {
    "amplitude": 0.01,
    "type": "random"
  }
}


### Lake Constance mesocosm — symmetric IC perturbation (Fig. 4.7 left panel)
{
  "type": "ObservationCollection",
  "featureOfInterest": {
    "type": "Feature",
    "geometry": { "type": "Point", "coordinates": [9.37, 47.65, -5.0] },
    "properties": {
      "name": "Mesocosm 3 — Lake Constance",
      "depth_m": 5,
      "water_body_type": "lake"
    }
  },
  "phenomenonTime": "2022-06-28/2022-07-01",
  "madeBySensor": "https://mwj.example.org/sensor/ctd-003",
  "hasMember": [
    {
      "observedProperty": "https://mwj.example.org/plankton/obs/phytoplanktonDensity",
      "hasResult": { "value": 2.15, "unit": "mg.dw/l" },
      "resultTime": "2022-07-01T08:00:00Z"
    },
    {
      "observedProperty": "https://mwj.example.org/plankton/obs/zooplanktonDensity",
      "hasResult": { "value": 2.92, "unit": "mg.dw/l" },
      "resultTime": "2022-07-01T08:00:00Z"
    },
    {
      "observedProperty": "https://mwj.example.org/plankton/obs/nutrientLevel",
      "hasResult": { "value": 2.5, "unit": "relative" },
      "resultTime": "2022-07-01T08:00:00Z"
    },
    {
      "observedProperty": "https://mwj.example.org/plankton/obs/fishPredationPressure",
      "hasResult": { "value": 0.4, "unit": "mg.dw/d/l" },
      "resultTime": "2022-07-01T08:00:00Z"
    }
  ],
  "perturbation": {
    "amplitude": 0.01,
    "type": "symmetric"
  }
}

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: PlanktonInitialObservation
description: "A SOSA ObservationCollection providing the initial state of a water
  body or mesocosm. Per the SSN/SOSA PROV alignment (W3C SSN 2017, \xA76.5):\n  -
  sosa:ObservationCollection is a container of sosa:Observation instances\n  - each
  sosa:Observation \u2291 prov:Activity\n  - each sosa:Sensor \u2291 prov:Agent  \u2192
  sosa:madeBySensor \u2291 prov:wasAssociatedWith\n  - each sosa:Result \u2291 prov:Entity
  \u2192 sosa:hasResult    \u2291 prov:wasGeneratedBy\n  - sosa:FeatureOfInterest
  \u2291 prov:Entity\nresultTime of the collection becomes t=0 of the simulation run.\n"
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
    description: "The sampled water body or mesocosm (sosa:FeatureOfInterest \u2291
      prov:Entity). Represented as a GeoJSON Feature.\n"
    required:
    - type
    - geometry
    properties:
      type:
        const: Feature
      geometry:
        type: object
        description: GeoJSON geometry of the sampling location
      properties:
        type: object
        properties:
          name:
            type: string
          depth_m:
            type: number
            minimum: 0
          water_body_type:
            type: string
            description: e.g. lake, mesocosm, coastal_shelf
    x-jsonld-id: http://www.w3.org/ns/sosa/hasFeatureOfInterest
  phenomenonTime:
    type: string
    description: "ISO 8601 time interval of the sampling campaign (e.g. '2022-06-28/2022-07-01').
      The sosa:phenomenonTime is the time the observed property values apply to the
      featureOfInterest \u2014 not necessarily when the observation was recorded.\n"
    x-jsonld-id: http://www.w3.org/ns/sosa/phenomenonTime
  madeBySensor:
    type: string
    description: "URI or name of the sensor / observer. sosa:madeBySensor \u2291 prov:wasAssociatedWith.
      The sensor is a sosa:Sensor \u2291 prov:Agent.\n"
    x-jsonld-id: http://www.w3.org/ns/sosa/madeBySensor
  hasMember:
    type: array
    description: 'The individual sosa:Observation instances that form this collection.
      Each member is itself a prov:Activity that generates one sosa:Result (prov:Entity)
      for one sosa:ObservableProperty of the featureOfInterest. Per the spec each
      Observation must have exactly one observedProperty and at least one hasResult.

      '
    minItems: 1
    items:
      type: object
      required:
      - observedProperty
      - hasResult
      - resultTime
      properties:
        observedProperty:
          type: string
          description: "URI of the sosa:ObservableProperty being measured. Use one
            of:\n  https://mwj.example.org/plankton/obs/phytoplanktonDensity\n  https://mwj.example.org/plankton/obs/zooplanktonDensity\n
            \ https://mwj.example.org/plankton/obs/nutrientLevel\n  https://mwj.example.org/plankton/obs/fishPredationPressure\n"
          x-jsonld-id: http://www.w3.org/ns/sosa/observedProperty
          x-jsonld-type: '@id'
        hasResult:
          type: object
          description: 'The sosa:Result of this individual Observation (prov:Entity
            wasGeneratedBy this Observation prov:Activity).

            '
          required:
          - value
          - unit
          properties:
            value:
              type: number
              description: Numeric measurement value
              x-jsonld-id: http://www.w3.org/ns/sosa/hasSimpleResult
            unit:
              type: string
              description: "Unit of measure. Expected values:\n  'mg.dw/l'     for
                X1, X2\n  'relative'    for N\n  'mg.dw/d/l'   for F\n"
              x-jsonld-id: http://qudt.org/schema/qudt/unit
          x-jsonld-id: http://www.w3.org/ns/sosa/hasResult
        resultTime:
          type: string
          format: date-time
          description: 'ISO 8601 timestamp when this individual observation was completed
            (sosa:resultTime). The resultTime of the first member becomes t=0 of the
            linked SimulationRun.

            '
          x-jsonld-id: http://www.w3.org/ns/sosa/resultTime
          x-jsonld-type: http://www.w3.org/2001/XMLSchema#dateTime
        usedProcedure:
          type: string
          description: "URI of the sosa:Procedure (measurement protocol) used for
            this individual observation. sosa:usedProcedure \u2291 prov:used. Optional;
            omit if using a generic field sampling protocol.\n"
          x-jsonld-id: http://www.w3.org/ns/sosa/usedProcedure
          x-jsonld-type: '@id'
    x-jsonld-id: http://www.w3.org/ns/sosa/hasMember
  perturbation:
    type: object
    description: 'Optional spatial perturbation applied to the homogeneous initial
      condition to seed pattern formation in the PDE simulation.

      '
    properties:
      amplitude:
        type: number
        description: Perturbation amplitude (fraction of steady-state value)
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

* YAML version: [schema.yaml](https://markuswilhelmjahn.github.io/bblocks-pdes/build/annotated/pdes/observation/schema.json)
* JSON version: [schema.json](https://markuswilhelmjahn.github.io/bblocks-pdes/build/annotated/pdes/observation/schema.yaml)


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
        },
        "usedProcedure": {
          "@id": "sosa:usedProcedure",
          "@type": "@id"
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
[context.jsonld](https://markuswilhelmjahn.github.io/bblocks-pdes/build/annotated/pdes/observation/context.jsonld)

## Sources

* [W3C SOSA/SSN Ontology](https://www.w3.org/TR/vocab-ssn/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/MarkusWilhelmJahn/bblocks-pdes](https://github.com/MarkusWilhelmJahn/bblocks-pdes)
* Path: `_sources/observation`

