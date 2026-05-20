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
