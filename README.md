# Plankton Model OGC Blocks

OGC Blocks implementation which uses the OGC standards SOSA, PROV and STAC combined to generate an interface for publishing the simulation results from a pytho-plankton and zoo-plankton interaction described by partial differential equations and numerically solved by using the Runge-Kutta method.

## Usage of Standards

```
Field observation
        │  X1₀, X2₀, N, F, location, time
        │  ↓  SOSA ObservationCollection
        │
        ├──────────────────────────────────────────┐
        │                                          │
  Parameters (α, c, γ, δ, …)              FiniteDifferenceMethod4PDES
        │                                          │
        └──────────── PROV Activity ───────────────┘
                           │ generates
                    timeseries.csv
                    phase_portrait.png
                           │
                     STAC Item  ←── discoverable, filterable by δ / scenario
```

## Folder Structure

```
bblocks-SOSAPROVSTAC/
├── bblocks-config.yaml                    ← identifier-prefix: mwj.pdes.
└── _sources/
    ├── parameters/
    │   ├── bblock.json
    │   ├── schema.yaml
    │   ├── context.jsonld
    │   ├── description.md
    │   └── examples.yaml
    ├── observation/
    │   ├── bblock.json
    │   ├── schema.yaml
    │   ├── context.jsonld
    │   ├── description.md
    │   └── examples.yaml
    ├── simulation-run/
    │   ├── bblock.json
    │   ├── schema.yaml
    │   ├── context.jsonld
    │   ├── description.md
    │   ├── examples.yaml
    │   └── shapes.ttl
    └── stac-result/
        ├── bblock.json
        ├── schema.yaml
        ├── context.jsonld
        ├── description.md
        └── examples.yaml
```

[General information on design and usage of OGC Blocks](https://github.com/opengeospatial/bblock-template/blob/master/USAGE.md)
