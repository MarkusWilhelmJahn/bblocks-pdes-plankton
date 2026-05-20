# Plankton RDA Simulation Run (PROV)

A **PROV `Activity`** capturing one execution of the Scheffer (1991)
phytoplankton/zooplankton RDA model via
[FiniteDifferenceMethod4PDES](https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES).

## Lineage graph

```
SOSA ObservationCollection ──prov:used──► SimulationRun (Activity)
Parameter set (Entity)     ──prov:used──►       │
FiniteDifferenceMethod4PDES (Agent) ─────────────┤ wasAssociatedWith
                                         prov:wasGeneratedBy
                              ┌──────────────────▼──────────────────────┐
                              │ X1(x,t) spatial field (CSV / NetCDF)     │
                              │ X2(x,t) spatial field                    │
                              │ Surface plots (PNG)                      │
                              │ Wave-number analysis plot (PNG)          │
                              └──────────────────────────────────────────┘
                                                 │ dcat:landingPage
                                           STAC Item
```

## Scenario classification

| `scenario` | D1 | D2 | v1x−v2x | Expected output |
|-----------|----|----|---------|-----------------|
| `ODE-*` | 0 | 0 | 0 | Time series X1(t), X2(t) |
| `1D-Turing-standing-pattern` | 10⁻⁵ | 2×10⁻³ | 0 | Stationary spatial pattern (Fig. 4.4) |
| `2D-Turing-standing-pattern` | 10⁻⁵ | 2×10⁻³ | 0 | 2D surface plot (Figs. 4.6, 4.7) |
| `1D-DIFICI-travelling-wave` | 10⁻⁵ | 10⁻⁵ | 0.01 | Moving population wave (Fig. 4.5) |
| `2D-DIFICI-travelling-wave` | 10⁻⁵ | 10⁻⁵ | 0.01 | 2D moving wave (Fig. 4.8) |

## Software

`wasAssociatedWith.id` = `https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES`
Local path: `/Users/markusjahn/Documents/GitHub/FiniteDifferenceMethod4PDES`
