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
