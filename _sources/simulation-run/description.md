# Plankton Simulation Procedure (SOSA)

A **sosa:Procedure** describing how to execute the Scheffer (1991)
phytoplankton/zooplankton RDA model in
[FiniteDifferenceMethod4PDES](https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES).

## SOSA alignment

`SimulationProcedure` is a `sosa:Procedure`. It is referenced from a
`sosa:ObservationCollection` (initial conditions) via `sosa:usedProcedure`.
Each execution produces a VTI time series catalogued as a STAC Item.

```
sosa:ObservationCollection  ──sosa:usedProcedure──► PlanktonSimulationProcedure
  (initial conditions,                                (sosa:Procedure)
   mwj.pdes.observation)                                    │
                                                    sosa:implementedBy
                                              FiniteDifferenceMethod4PDES
                                                    (prov:Agent)
```

## Model equations

```
dX₁/dt = alpha · N/(HN+N) · X₁  −  c · X₁²  −  gamma · X₁/(H₁+X₁) · X₂
dX₂/dt = e · gamma · X₁/(H₁+X₁) · X₂  −  delta · X₂  −  F · X₂²/(H₂²+X₂²)
```

For PDE runs, spatial operators are added:

```
∂Xᵢ/∂t = (local kinetics)  −  vᵢ · ∇Xᵢ  +  Dᵢ · ∇²Xᵢ
```

## Scenario classification

| `scenario` | D₁ | D₂ | v₁ₓ−v₂ₓ | Pattern |
|---|---|---|---|---|
| `ODE-stable-node` | 0 | 0 | 0 | Decaying to fixed point |
| `ODE-stable-focus` | 0 | 0 | 0 | Damped oscillations |
| `ODE-limit-cycle` | 0 | 0 | 0 | Sustained oscillations |
| `1D-Turing-standing-pattern` | 10⁻⁵ | 2×10⁻³ | 0 | Stationary pattern (Fig. 4.4) |
| `2D-Turing-standing-pattern` | 10⁻⁵ | 2×10⁻³ | 0 | 2D surface (Figs. 4.6, 4.7) |
| `1D-DIFICI-travelling-wave` | 10⁻⁵ | 10⁻⁵ | 0.01 | Moving wave (Fig. 4.5) |
| `2D-DIFICI-travelling-wave` | 10⁻⁵ | 10⁻⁵ | 0.01 | 2D moving wave (Fig. 4.8) |
