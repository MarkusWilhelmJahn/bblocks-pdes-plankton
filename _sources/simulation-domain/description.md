# Plankton Simulation Domain (VTK Image Data Grid)

OGC spatial description of the **VTK Image Data** (`vtkImageData`) structured grid used as the plankton RDA simulation domain in FiniteDifferenceMethod4PDES.

## Two domain types

### 1. PDE spatial domain (`domainType: spatial-pde`)
The physical space over which the phytoplankton/zooplankton reaction-diffusion-advection PDE is solved. Grid cells hold X1 (phytoplankton) and X2 (zooplankton) values at each timestep. Geometry is a LineString (1D) or Polygon (2D).

### 2. ODE phase-portrait domain (`domainType: phase-portrait-ode`)
For pure ODE runs, `Room.getDGL()` maps the plankton state-space onto a 3D voxel grid. The two axes represent X1 and X2 values, not physical space.

## VTK grid structure

```
file: <runName>_<step>.vti

VTK Image Data:
  WholeExtent: 0 nx 0 ny 0 nz
  Origin:      0 0 0
  Spacing:     dx dy dz

PointData arrays:
  "phase Phytoplankton"   ← X1 at each grid point
  "phase Zooplankton"     ← X2 at each grid point
  "local Phytoplankton"   ← local spatial statistics
  "local Zooplankton"
  "global Phytoplankton"  ← global spatial statistics
  "global Zooplankton"

Cell index: j = x + y·nx + z·nx·ny
Position:   p = origin + [x·dx, y·dy, z·dz]
```

## Initial conditions

**Step 1 (`<runName>_1.vti`) is the initial condition** — its point-data arrays contain the initial spatial distribution of phytoplankton and zooplankton. This file is referenced by `vtiSeries.initFile` and linked to the `mwj.pdes.observation` block.
