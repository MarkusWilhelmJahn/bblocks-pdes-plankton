# Partial Differential Equation Systems OGC Blocks

OGC Blocks implementation which uses the OGC standards SOSA, PROV and STAC combined to generate an interface for publishing the simulation results from a pytho-plankton and zoo-plankton interaction described by partial differential equations and numerically solved by using the Runge-Kutta method.


## About this Register:

### **Who is this register for?**

### **What does this register contain?**

### **When should this register be used?**

### **Where does this register fit?**

### **Why does this register exist?**

### **How is this register structured and implemented?**

### **For Developers**
*   **[GitHub Repository](https://github.com/ogcincubator/bblocks-SOSAPROVSTAC)**
*   **[OGC Blocks Documentation](https://ogcincubator.github.io/bblocks-docs/)**
*   **[OGC Blocks Tutorials](https://ogcincubator.github.io/bblocks-tutorial/)**
*   **[OGC Blocks Examples](https://ogcincubator.github.io/bblocks-examples/)**


## Building Blocks

### `mwj.pdes.parameters` — Scheffer Phytoplankton-Zooplankton RDA Parameters

**Type:** schema

Typed parameter set for the Scheffer (1991) phytoplankton/zooplankton reaction-diffusion-advection model (Malchow, GlgbasModII WS2010, eqs. 4.64/4.65). Covers reaction parameters, diffusion coefficients and advection velocities.

### `mwj.pdes.observation` — Plankton Initial Observation (SOSA)

**Type:** schema

SOSA ObservationCollection providing initial phytoplankton density X1(x,0), zooplankton density X2(x,0), nutrient level N and fish predation rate F for a RDA simulation run.

### `mwj.pdes.simulation-run` — Plankton RDA Simulation Run (PROV)

**Type:** schema

PROV-O Activity capturing a single FDM simulation of the Scheffer phytoplankton/zooplankton RDA model, linking SOSA initial observations and parameters to output spatial fields and time series.

### `mwj.pdes.stac-result` — Plankton RDA Simulation STAC Item

**Type:** schema

STAC 1.0 Item cataloguing spatial field outputs of a plankton RDA simulation run, filterable by scenario, diffusion ratio and advection velocity.

