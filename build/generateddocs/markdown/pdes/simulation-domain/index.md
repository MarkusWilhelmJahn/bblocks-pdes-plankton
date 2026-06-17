
# Plankton Simulation Domain (VTK Image Data Grid) (Schema)

`mwj.pdes.simulation-domain` *v0.1*

OGC spatial description of the VTK Image Data (vtkImageData) grid used as the plankton RDA simulation domain in FiniteDifferenceMethod4PDES. The first VTI timestep file (Plankton_1.vti) provides initial conditions for the sosa:ObservationCollection.

[*Status*](http://www.opengis.net/def/status): Experimental

## Description

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

## Examples

### Plankton 1D PDE spatial domain (Plankton_1.vti initial conditions)
{
  "type": "SimulationDomain",
  "id": "https://mwj.example.org/pdes/domain/plankton-1d-100cm",
  "label": "Plankton-1D-100cm",
  "domainType": "spatial-pde",
  "dimensions": { "nx": 200, "ny": 1, "nz": 1 },
  "spacing":    { "dx": 0.5, "dy": 1.0, "dz": 1.0 },
  "origin":     { "x": 0.0, "y": 0.0, "z": 0.0 },
  "arrayNames": [
    "phase Phytoplankton", "phase Zooplankton",
    "local Phytoplankton", "local Zooplankton",
    "global Phytoplankton", "global Zooplankton"
  ],
  "vtiSeries": {
    "runName":    "set_01_Plankton",
    "stepCount":  210,
    "initFile":   "data/set_01_Plankton_1.vti",
    "dataFolder": "data/"
  },
  "geometry": {
    "type": "LineString",
    "coordinates": [[0.0, 0.0], [100.0, 0.0]]
  },
  "crs": "model-units",
  "boundingBox": {
    "min": [0.0, 0.0, 0.0],
    "max": [100.0, 0.0, 0.0]
  }
}


### Plankton 2D PDE spatial domain (100x100 cm)
{
  "type": "SimulationDomain",
  "id": "https://mwj.example.org/pdes/domain/plankton-2d-100x100cm",
  "label": "Plankton-2D-100x100cm",
  "domainType": "spatial-pde",
  "dimensions": { "nx": 100, "ny": 100, "nz": 1 },
  "spacing":    { "dx": 1.0, "dy": 1.0, "dz": 1.0 },
  "origin":     { "x": 0.0, "y": 0.0, "z": 0.0 },
  "arrayNames": [
    "phase Phytoplankton", "phase Zooplankton",
    "local Phytoplankton", "local Zooplankton",
    "global Phytoplankton", "global Zooplankton"
  ],
  "geometry": {
    "type": "Polygon",
    "coordinates": [[[0.0,0.0],[100.0,0.0],[100.0,100.0],[0.0,100.0],[0.0,0.0]]]
  },
  "crs": "model-units",
  "boundingBox": {
    "min": [0.0, 0.0, 0.0],
    "max": [100.0, 100.0, 0.0]
  }
}


### Plankton ODE phase-portrait domain
{
  "type": "SimulationDomain",
  "id": "https://mwj.example.org/pdes/domain/plankton-phase-portrait",
  "label": "Plankton-PhasePortrait",
  "domainType": "phase-portrait-ode",
  "dimensions": { "nx": 52, "ny": 26, "nz": 13 },
  "spacing":    { "dx": 1.0, "dy": 1.0, "dz": 1.0 },
  "origin":     { "x": 0.0, "y": 0.0, "z": 0.0 },
  "arrayNames": [
    "phase Phytoplankton", "phase Zooplankton"
  ],
  "geometry": {
    "type": "Point",
    "coordinates": [0.0, 0.0]
  },
  "crs": "model-units",
  "boundingBox": {
    "min": [0.0, 0.0, 0.0],
    "max": [1.0, 1.0, 1.0]
  }
}

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: PlanktonSimulationDomain
description: "Spatial description of the VTK Image Data (vtkImageData) grid used as
  the plankton RDA simulation domain in FiniteDifferenceMethod4PDES.\nTwo domain types
  are supported:\n1. **PDE spatial domain** (`domainType: spatial-pde`) \u2014 the
  physical space\n   over which the reaction-diffusion-advection PDE is solved. Each
  grid cell\n   holds phytoplankton (X1) and zooplankton (X2) values at one timestep.\n
  \  Geometry is a LineString (1D) or Polygon (2D).\n\n2. **ODE phase-portrait domain**
  (`domainType: phase-portrait-ode`) \u2014 for\n   pure ODE runs, the VTK grid maps
  state-space onto a 3D voxel grid via\n   Room.getDGL(). The geometry is abstract
  (no real-world CRS).\n\nVTK Image Data grid structure (from Room.java):\n  j = x
  + y * dim[0] + z * dim[0] * dim[1]\n  position[x,y,z] = origin + [x * spacing[0],
  y * spacing[1], z * spacing[2]]\n\nVTI point-data array names follow the convention:\n
  \ \"phase Phytoplankton\", \"phase Zooplankton\"   (phase-space arrays)\n  \"local
  Phytoplankton\", \"local Zooplankton\"   (local statistics)\n  \"global Phytoplankton\",
  \"global Zooplankton\" (global statistics)\n"
type: object
required:
- type
- domainType
- dimensions
- spacing
- origin
- arrayNames
properties:
  type:
    const: SimulationDomain
  id:
    type: string
    description: URI identifying this domain
    x-jsonld-id: '@id'
  label:
    type: string
    description: Human-readable label, e.g. 'Plankton-1D-100cm'
    x-jsonld-id: http://www.w3.org/2000/01/rdf-schema#label
  domainType:
    type: string
    enum:
    - spatial-pde
    - phase-portrait-ode
    description: '''spatial-pde'': real spatial domain of a PDE simulation. ''phase-portrait-ode'':
      abstract state-space grid for ODE visualisation.

      '
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#domainType
  dimensions:
    type: object
    required:
    - nx
    - ny
    - nz
    description: Number of grid points in each direction (vtkImageData WholeExtent)
    properties:
      nx:
        type: integer
        minimum: 1
      ny:
        type: integer
        minimum: 1
      nz:
        type: integer
        minimum: 1
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#gridDimensions
  spacing:
    type: object
    required:
    - dx
    - dy
    - dz
    description: Grid cell spacing in each direction (VTK Spacing attribute)
    properties:
      dx:
        type: number
        minimum: 0
      dy:
        type: number
        minimum: 0
      dz:
        type: number
        minimum: 0
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#gridSpacing
  origin:
    type: object
    required:
    - x
    - y
    - z
    description: Grid origin coordinates (VTK Origin attribute)
    properties:
      x:
        type: number
      y:
        type: number
      z:
        type: number
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#gridOrigin
  arrayNames:
    type: array
    minItems: 1
    items:
      type: string
    description: 'Names of the VTK point-data arrays in the VTI files. For the plankton
      model: ''phase Phytoplankton'', ''phase Zooplankton'', ''local Phytoplankton'',
      ''local Zooplankton'', etc.

      '
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#stateVariableArrays
  vtiSeries:
    type: object
    description: References to the VTI time-series files.
    required:
    - runName
    - stepCount
    - initFile
    properties:
      runName:
        type: string
        description: 'Run name prefix: <runName>_<step>.vti'
      stepCount:
        type: integer
        minimum: 1
        description: Total number of timestep files in the series
      initFile:
        type: string
        description: Path to the initial-condition VTI file (step = 1)
      dataFolder:
        type: string
        description: Folder containing all VTI files
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#vtiSeries
  geometry:
    type: object
    description: 'GeoJSON geometry of the simulation domain extent. LineString for
      1D PDE, Polygon for 2D PDE, Point for ODE phase-portrait.

      '
    properties:
      type:
        type: string
        enum:
        - Point
        - LineString
        - Polygon
      coordinates: {}
    x-jsonld-id: http://www.opengis.net/ont/geosparql#hasGeometry
  crs:
    type: string
    description: 'Coordinate reference system. Use ''model-units'' for abstract numerical
      domains. Use an EPSG URI for real geographic domains.

      '
    default: model-units
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#coordinateReferenceSystem
  boundingBox:
    type: object
    description: Model-space bounding box
    properties:
      min:
        type: array
        items:
          type: number
        minItems: 3
        maxItems: 3
      max:
        type: array
        items:
          type: number
        minItems: 3
        maxItems: 3
    x-jsonld-id: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#boundingBox
x-jsonld-extra-terms:
  SimulationDomain: http://www.opengis.net/ont/geosparql#SpatialObject
x-jsonld-prefixes:
  geo: http://www.opengis.net/ont/geosparql#
  rdfs: http://www.w3.org/2000/01/rdf-schema#
  mwj: https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#
  xsd: http://www.w3.org/2001/XMLSchema#

```

Links to the schema:

* YAML version: [schema.yaml](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/simulation-domain/schema.json)
* JSON version: [schema.json](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/simulation-domain/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "SimulationDomain": "geo:SpatialObject",
    "id": "@id",
    "label": "rdfs:label",
    "domainType": "mwj:domainType",
    "dimensions": "mwj:gridDimensions",
    "spacing": "mwj:gridSpacing",
    "origin": "mwj:gridOrigin",
    "arrayNames": "mwj:stateVariableArrays",
    "vtiSeries": "mwj:vtiSeries",
    "geometry": "geo:hasGeometry",
    "crs": "mwj:coordinateReferenceSystem",
    "boundingBox": "mwj:boundingBox",
    "geo": "http://www.opengis.net/ont/geosparql#",
    "rdfs": "http://www.w3.org/2000/01/rdf-schema#",
    "mwj": "https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES/ont#",
    "xsd": "http://www.w3.org/2001/XMLSchema#",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://markuswilhelmjahn.github.io/bblocks-pdes-plankton/build/annotated/pdes/simulation-domain/context.jsonld)

## Sources

* [VTK Image Data format](https://vtk.org/wp-content/uploads/2015/04/file-formats.pdf)
* [FiniteDifferenceMethod4PDES](https://github.com/MarkusWilhelmJahn/FiniteDifferenceMethod4PDES)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/MarkusWilhelmJahn/bblocks-pdes-plankton](https://github.com/MarkusWilhelmJahn/bblocks-pdes-plankton)
* Path: `_sources/simulation-domain`

