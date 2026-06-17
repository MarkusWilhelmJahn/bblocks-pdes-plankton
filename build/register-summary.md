# Plankton RDA OGC Building Block Example

A self-contained OGC Building Block example illustrating how SOSA, PROV-O
and STAC can be combined to describe a simulation model, its initial
conditions, parameter sets and result cataloguing — using the Scheffer (1991)
phytoplankton/zooplankton RDA model as a concrete case.


## About this Register

This register is an **example** showing how to apply OGC standards to
equation-based ecological simulation models. It is intentionally
self-contained and does not depend on any other building block register.

### What does this register contain?

- **simulation-run**: A `sosa:Procedure` describing how to execute the
  Scheffer RDA model in FiniteDifferenceMethod4PDES
- **observation**: A `sosa:ObservationCollection` providing initial
  phytoplankton and zooplankton conditions
- **parameters**: Typed Scheffer RDA parameter set
- **stac-result**: STAC Item cataloguing simulation outputs

### For Developers
* **[GitHub Repository](https://github.com/MarkusWilhelmJahn/bblocks-pdes-plankton)**
* **[OGC Blocks Documentation](https://ogcincubator.github.io/bblocks-docs/)**


## Building Blocks

### `mwj.pdes.parameters` — Scheffer Phytoplankton-Zooplankton RDA Parameters

**Type:** schema

Typed parameter set for the Scheffer (1991) phytoplankton/zooplankton reaction-diffusion-advection model. Covers reaction parameters, diffusion coefficients and advection velocities.

### `mwj.pdes.simulation-domain` — Plankton Simulation Domain (VTK Image Data Grid)

**Type:** schema

OGC spatial description of the VTK Image Data (vtkImageData) grid used as the plankton RDA simulation domain in FiniteDifferenceMethod4PDES. The first VTI timestep file (Plankton_1.vti) provides initial conditions for the sosa:ObservationCollection.

### `mwj.pdes.observation` — Plankton Initial Observation (SOSA)

**Type:** schema

SOSA ObservationCollection providing initial phytoplankton density X1(x,0) and zooplankton density X2(x,0) for a plankton RDA simulation run. The featureOfInterest references the SimulationDomain (VTK Image Data grid).

### `mwj.pdes.simulation-run` — Plankton Simulation Procedure (SOSA)

**Type:** schema

A sosa:Procedure describing how to execute the Scheffer (1991) phytoplankton/zooplankton RDA model in FiniteDifferenceMethod4PDES, specifying the model class, numerical solver, spatial domain and time discretisation.

### `mwj.pdes.stac-result` — Plankton RDA Simulation STAC Item

**Type:** schema

STAC 1.0 Item cataloguing spatial field outputs of a plankton RDA simulation run, filterable by scenario, diffusion ratio and advection velocity.

