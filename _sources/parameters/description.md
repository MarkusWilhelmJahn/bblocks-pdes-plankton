# Scheffer Phytoplankton-Zooplankton RDA Parameters

Parameter schema for the two-species **reaction-diffusion-advection** model
from Malchow, *Equation-Based Models II* (WS 2010/11), equations 4.64/4.65.
Original local kinetics from Scheffer (1991).

## Governing equations

```
dX₁/dt = α · N/(Hₙ+N) · X₁  −  c · X₁²  −  γ · X₁/(H₁+X₁) · X₂
          − v₁ · ∇X₁  +  D₁ · ∇²X₁

dX₂/dt = e · γ · X₁/(H₁+X₁) · X₂  −  δ · X₂
          − F · X₂²/(H₂²+X₂²)
          − v₂ · ∇X₂  +  D₂ · ∇²X₂
```

where:
- X₁ = phytoplankton density [mg dw l⁻¹]
- X₂ = zooplankton density [mg dw l⁻¹]
- X₁/(H₁+X₁) — Holling Type II functional response (zooplankton grazing)
- X₂²/(H₂²+X₂²) — Holling Type III functional response (fish predation)

## Default parameter values (Malchow 2000 / Scheffer 1991)

| Parameter | Value | Unit | Description |
|-----------|-------|------|-------------|
| α | 0.5 | d⁻¹ | Max phytoplankton growth rate |
| c | 0.05 | mg dw⁻¹ l d⁻¹ | Intraspecific competition |
| γ | 0.4 | d⁻¹ | Max zooplankton grazing rate |
| H₁ | 0.6 | mg dw l⁻¹ | Half-saturation constant — grazing (Holling II) |
| H₂ | 5.0 | mg dw l⁻¹ | Half-saturation constant — fish predation (Holling III) |
| Hₙ | 1.0 | rel. | Half-saturation constant — nutrient uptake |
| e | 0.6 | — | Zooplankton assimilation efficiency |
| **δ** | **0.175** | d⁻¹ | Zooplankton mortality rate (primary bifurcation parameter) |
| N | 2.5 | rel. | Nutrient level (external control) |
| F | 0.4 | mg dw d⁻¹ l⁻¹ | Fish predation pressure (external control) |
| D₁ | 10⁻⁵ | cm² d⁻¹ | Phytoplankton diffusivity |
| D₂ | 10⁻⁵ | cm² d⁻¹ | Zooplankton diffusivity |

## Spatial scenarios (Figs. 4.4–4.8)

| Scenario | D₁ | D₂ | v₁ₓ − v₂ₓ | Outcome |
|----------|----|----|------------|---------|
| ODE only | 0 | 0 | 0 | Local oscillations / fixed points |
| Turing structures | 10⁻⁵ | 2×10⁻³ | 0 | Standing spatial patterns |
| Travelling waves (DIFICI) | 10⁻⁵ | 10⁻⁵ | 0.01 | Propagating population waves |

Turing instability condition: D₂/D₁ ≳ 100

DIFICI condition: |v₁ₓ − v₂ₓ| > 0 with D₁ = D₂

## Implemented in

[FiniteDifferenceMethod4PDES](https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES)
— explicit finite difference solver for 1D and 2D domains with zero-flux
and periodic boundary conditions.
