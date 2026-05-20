# Scheffer Phytoplankton-Zooplankton RDA Parameters

Parameter schema for the two-species **reaction-diffusion-advection** model
from Malchow, *Gleichungsbasierte Modelle II* (WS 2010/11), equations 4.64/4.65.
Original local kinetics from Scheffer (1991).

## Governing equations

```
∂X1/∂t = α·N/(HN+N)·X1 − c·X1² − γ·X1/(H1+X1)·X2 − v1·∇X1 + D1·∇²X1
∂X2/∂t = e·γ·X1/(H1+X1)·X2 − δ·X2 − F·X2²/(H2²+X2²) − v2·∇X2 + D2·∇²X2
```

- **X1** = phytoplankton density [mg.dw l⁻¹]
- **X2** = zooplankton density [mg.dw l⁻¹]
- Holling Type II functional response for zooplankton grazing
- Holling Type III for fish predation on zooplankton

## Default parameter values (Malchow 2000 / Scheffer 1991)

| Parameter | Value | Unit | Description |
|-----------|-------|------|-------------|
| α | 0.5 | d⁻¹ | Max phytoplankton growth rate |
| c | 0.05 | mg.dw⁻¹ l d⁻¹ | Intraspecific competition |
| γ | 0.4 | d⁻¹ | Max grazing rate |
| H1 | 0.6 | mg.dw l⁻¹ | Half-sat grazing (Holling II) |
| H2 | 5.0 | mg.dw l⁻¹ | Half-sat fish predation (Holling III) |
| HN | 1.0 | rel. | Half-sat nutrient uptake |
| e | 0.6 | — | Assimilation efficiency |
| **δ** | **0.175** | **d⁻¹** | **Zooplankton mortality (bifurcation param)** |
| N | 2.5 | rel. | Nutrient level (control) |
| F | 0.4 | mg.dw d⁻¹ l⁻¹ | Fish predation (control) |

## Spatial scenarios (from Figs. 4.4–4.8 of script)

| Scenario | D1 | D2 | v1x−v2x | Outcome |
|----------|----|----|---------|---------|
| Turing structures | 10⁻⁵ | 2×10⁻³ | 0 | Standing spatial patterns |
| Travelling waves (DIFICI) | 10⁻⁵ | 10⁻⁵ | 0.01 | Propagating population waves |
| ODE only | 0 | 0 | 0 | Local oscillations / fixed points |

## Implemented in

[FiniteDifferenceMethod4PDES](https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES)
— explicit finite difference solver for 1D and 2D domains with zero-flux
and periodic boundary conditions.
