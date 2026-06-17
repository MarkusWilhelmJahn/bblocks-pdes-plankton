# bblocks-pdes-plankton — OGC Building Block Example

A self-contained OGC Building Block example showing how SOSA, PROV-O and STAC 1.0
can be combined to describe a simulation model, its initial conditions, parameter
sets, spatial domain and result cataloguing — using the **Scheffer (1991)
phytoplankton/zooplankton reaction-diffusion-advection (RDA) model** as a
concrete case.

This register is an **example**, not a library. It is intentionally independent
of any other building block register and can be built standalone with `sh build.sh`.

---

## The model

The Scheffer (1991) / Malchow plankton model describes the coupled dynamics of
phytoplankton $X_1$ and zooplankton $X_2$ in a water body:

$$
\frac{\partial X_1}{\partial t} = a \frac{N}{H_N + N} X_1 - c X_1^2 - b \frac{X_1}{H_1 + X_1} X_2 - \mathbf{v}_1 \cdot \nabla X_1 + D_1 \nabla^2 X_1
$$

$$
\frac{\partial X_2}{\partial t} = d \cdot b \frac{X_1}{H_1 + X_1} X_2 - e X_2 - F \frac{X_2^2}{H_2^2 + X_2^2} - \mathbf{v}_2 \cdot \nabla X_2 + D_2 \nabla^2 X_2
$$

Depending on parameters, the model produces:

| Scenario | Condition | Spatial outcome |
|---|---|---|
| ODE stable focus | Default $e = 0.175$ | Damped oscillations |
| ODE limit cycle | $e \lesssim 0.15$ | Sustained oscillations |
| Turing patterns | $D_2/D_1 \gtrsim 100$ | Standing spatial structures |
| DIFICI travelling waves | $D_1 = D_2$, $\mathbf{v}_1 \neq \mathbf{v}_2$ | Propagating wave fronts |

Implemented in
[FiniteDifferenceMethod4PDES](https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES)
using an explicit finite difference scheme on `vtkImageData` grids.

---

## Standards used

| Standard | Role in this register |
|---|---|
| [SOSA/SSN](https://www.w3.org/TR/vocab-ssn/) | `sosa:Procedure` for the simulation procedure; `sosa:ObservationCollection` for initial plankton conditions |
| [PROV-O](https://www.w3.org/TR/prov-o/) | Provenance linking outputs back to procedure and observations |
| [STAC 1.0](https://stacspec.org/) | Cataloguing VTI time-series outputs with `plankton-rda:*` extension properties |
| [GeoSPARQL](https://www.ogc.org/standards/geosparql) | Spatial description of the VTK Image Data grid |
| [JSON-LD](https://www.w3.org/TR/json-ld11/) | Linked data context for all blocks |

---

## How the standards fit together

```
Field observation (Lake Constance mesocosm)
        │
        │  X₁₀ = 3.116 mg dw/l   (phytoplankton)
        │  X₂₀ = 1.870 mg dw/l   (zooplankton)
        │  N = 2.5, F = 0.4
        │
        ▼
sosa:ObservationCollection          ←  mwj.pdes.observation
  madeBySensor: ctd-003
  usedProcedure ──────────────────► sosa:Procedure (PlanktonSimulationProcedure)
                                           │         ←  mwj.pdes.simulation-run
                                           │  modelClass: model.lokal.o2.Plankton
                                           │  numericalSolver: jEuler
                                           │  simulationDomain ──► SimulationDomain
                                           │                        ←  mwj.pdes.simulation-domain
                                           │  parameters ──────────► PlanktonRDAParameters
                                           │                         ←  mwj.pdes.parameters
                                           │
                                    prov:wasGeneratedBy
                                           │
                                           ▼
                               set_01_Plankton_1.vti
                               set_01_Plankton_2.vti   (VTK ImageData time series)
                               …
                               set_01_Plankton_N.vti
                                           │
                                           ▼
                                     STAC Item          ←  mwj.pdes.stac-result
                                  (filterable by scenario,
                                   D_ratio, v_rel, δ, …)
```

---

## Register structure

```
bblocks-pdes-plankton/
├── bblocks-config.yaml              ← identifier-prefix: mwj.pdes.
└── _sources/
    ├── parameters/                  ← Scheffer RDA parameter set (α,c,γ,H₁,H₂,HN,e,δ,F,N,D₁,D₂,…)
    │   ├── bblock.json
    │   ├── schema.yaml
    │   ├── context.jsonld
    │   ├── description.md
    │   └── examples.yaml
    ├── simulation-domain/           ← VTK Image Data grid for plankton domain
    │   ├── bblock.json
    │   ├── schema.yaml
    │   ├── context.jsonld
    │   ├── description.md
    │   └── examples.yaml
    ├── observation/                 ← Initial phytoplankton / zooplankton conditions
    │   ├── bblock.json
    │   ├── schema.yaml
    │   ├── context.jsonld
    │   ├── description.md
    │   └── examples.yaml
    ├── simulation-run/              ← Plankton simulation procedure (sosa:Procedure)
    │   ├── bblock.json
    │   ├── schema.yaml
    │   ├── context.jsonld
    │   ├── description.md
    │   └── examples.yaml
    └── stac-result/                 ← STAC Item with plankton-rda:* extension
        ├── bblock.json
        ├── schema.yaml
        ├── context.jsonld
        ├── description.md
        └── examples.yaml
```

---

## Block dependencies (within this register)

```
parameters
    ↑
simulation-domain
    ↑
observation ──────────────► parameters
    ↑
simulation-run ────────────► simulation-domain, observation, parameters
    ↑
stac-result ───────────────► simulation-run, observation, simulation-domain
```

---

## Building locally

```bash
sh build.sh
```

Requires Docker. Output is written to `build-local/`.

---

## Mathematical notation

All equations follow the notation of:
- *Gleichungsbasierte Modelle II* (GbM II), §4.6 — Prof. H. Malchow, Universität Osnabrück (WS 2010/11)
- Scheffer, M. (1991). Oikos 62:271–282.
- Malchow, H. (2000). J. Theor. Biol. 204:639–647.

---

## Related registers

- **[bblocks-pdes](https://github.com/MarkusWilhelmJahn/bblocks-pdes)** —
  library register covering all 22 ODE/PDE models in FiniteDifferenceMethod4PDES,
  reusable common blocks and Holling component blocks

---

[OGC Building Blocks documentation](https://ogcincubator.github.io/bblocks-docs/)
