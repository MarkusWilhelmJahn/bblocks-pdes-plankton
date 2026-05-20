# Plankton RDA Simulation STAC Item

A **STAC 1.0 Feature** cataloguing the spatial field outputs of one plankton
RDA simulation run, using the `plankton-rda` extension namespace.

## Key searchable extension properties

| Property | Type | Notes |
|----------|------|-------|
| `plankton-rda:model` | string | Always `"Scheffer1991-RDA"` |
| `plankton-rda:scenario` | enum | Attractor / pattern type |
| `plankton-rda:spatial_dimensions` | 1 or 2 | Dimensionality of the run |
| `plankton-rda:D_ratio` | number | D2/D1 — Turing instability threshold ≈ 100 |
| `plankton-rda:v_rel` | number | \|v1x − v2x\| — DIFICI threshold > 0 |
| `plankton-rda:delta` | number | Zooplankton mortality [d⁻¹] |
| `plankton-rda:boundary_condition` | enum | zero-flux / periodic |
| `plankton-rda:provenance` | URI | → PROV `SimulationRun` |
| `plankton-rda:observation` | URI | → SOSA `ObservationCollection` |

## Link relations

| `rel` | Target |
|-------|--------|
| `derived_from` | PROV `SimulationRun` JSON |
| `related` | SOSA `ObservationCollection` JSON |
| `software` | FiniteDifferenceMethod4PDES GitHub repo |

## Filtering via STAC API

```
# All Turing runs in 2D
GET /search?filter=plankton-rda:scenario='2D-Turing-standing-pattern'

# Runs with strong differential advection
GET /search?filter=plankton-rda:v_rel>0.005

# Runs with diffusivity ratio above Turing threshold
GET /search?filter=plankton-rda:D_ratio>=100
```
