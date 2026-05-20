# Plankton Initial Observation (SOSA)

A **SOSA `ObservationCollection`** providing the initial state of a water body
for a phytoplankton/zooplankton RDA simulation run.

## Observable properties

| Field | Unit | Notes |
|-------|------|-------|
| `X1_initial` | mg.dw l⁻¹ | Phytoplankton density at t=0, typically the ODE steady state |
| `X2_initial` | mg.dw l⁻¹ | Zooplankton density at t=0 |
| `N_observed` | relative | Nutrient level → control parameter N |
| `F_observed` | mg.dw d⁻¹ l⁻¹ | Fish predation → control parameter F |
| `perturbation_amplitude` | — | Small noise amplitude added to seed patterns |
| `perturbation_type` | enum | random / symmetric / asymmetric / none |

## Initial condition convention

The model is initialised at the spatially homogeneous steady state
(X1ˢ, X2ˢ) of the ODE system plus a small perturbation
to trigger spatial pattern formation:

```
X1(x, 0) = X1_initial + ε · ξ(x)
X2(x, 0) = X2_initial + ε · ξ(x)
```

where ξ is the perturbation field and ε = `perturbation_amplitude`.

## Linking to simulation

`resultTime` becomes **t = 0** and is referenced from the `SimulationRun`
via `prov:used`.
