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
