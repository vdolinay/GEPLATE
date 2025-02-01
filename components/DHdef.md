# Hot Water Distribution System Model Structure

## Overview
The hot water distribution system model is structured in a relational format but is primarily defined using JSON. A software layer ensures the validity of the model. For complex systems, the model can be stored in a database, from which JSON configurations can be generated.

## Key Objects in the Model

### 1. **Network Metadata**
- Contains general information about the network, including its name and reference URIs.
- Includes a list of service URIs that the system components communicate with.
- **Properties**:
  - `name` (string) - Name of the network
  - `uriContainer` (array) - List of service URIs, each containing:
    - `id` (string) - Unique identifier
    - `uri` (string) - Reference URL

**Example JSON:**
```json
{
  "name": "District Heating System",
  "uriContainer": [
    { "id": "CON1", "uri": "http://example.com/api" }
  ]
}
```

### 2. **Prosumers (Producers and Consumers)**
- Represents entities in the system that can either **consume** or **produce** heat.
- Some prosumers, such as **accumulators**, can alternate between consumption and production depending on the system state.
- **Properties**:
  - `id` (string) - Unique identifier
  - `comment` (string) - Optional description of the prosumer

**Example JSON:**
```json
{
  "prosumers": [
    { "id": "P1", "comment": "Heat Producer" },
    { "id": "C1", "comment": "Heat Consumer" },
    { "id": "A1", "comment": "Accumulator" }
  ]
}
```

### 3. **Nodes**
- Defines junction points in the pipeline network.
- Nodes connect different pipe sections and allow branching within the distribution system.
- **Properties**:
  - `id` (string) - Unique identifier

**Example JSON:**
```json
{
  "nodes": [
    { "id": "N1" },
    { "id": "N2" }
  ]
}
```

### 4. **Pipe Sections**
- Represents segments of the pipeline connecting nodes and prosumers.
- Each section is uniquely identified and must contain at least one pipe definition.
- **Properties**:
  - `id` (string) - Unique identifier
  - `begin` (string) - Identifier of the starting node or prosumer
  - `end` (string) - Identifier of the ending node or prosumer
  - `pipes` (array) - Contains one or more pipe objects specifying the physical characteristics of the section.

**Example JSON:**
```json
{
  "pipeSections": [
    {
      "id": "S1", "begin": "P1", "end": "N1",
      "pipes": [
        {
          "length": 100.0,
          "diameter": 0.5,
          "placement": 1
        }
      ]
    }
  ]
}
```

### 5. **Pipes**
- Represents individual pipes within a section.
- Each pipe has specific parameters including:
  - **Thermal and Physical Properties**:
    - `length` (float) - Pipe length in meters
    - `diameter` (float) - Internal pipe diameter in meters
    - `placement` (integer) - Installation classification
    - `Properties` (object) - Optional thermal properties
    - `k` (float) - Correction factor for heat loss calculation.
    - `kHot` (float) - Correction factor for heat loss in the heating branch.
    - `kCold` (float) - Correction factor for heat loss in the return branch.

  - **Usage of kHot and kCold**:
    - When a pipe section serves as a blueprint for both the heating and return branches, `kHot` and `kCold` allow for independent correction of heat loss.
    - This differentiation accounts for variations in thermal losses due to temperature differences and flow direction.
    - If not explicitly defined, `k` is applied uniformly to both branches.

**Example JSON:**
```json
{
  "pipes": [
    {
      "length": 100.0,
      "diameter": 0.5,
      "placement": 1,
      "Properties": {
        "k": 1.001,
        "kHot": 1.05,
        "kCold": 0.98,
        "insulationThermalConductivity": "Pre-insulated"
      }
    }
  ]
}
```

### 6. **Default Parameters and Correction Factors**
- **Pipe Defaults**: Provides default values for common thermal and physical properties such as:
  - `NumParams` (object) - Predefined technical or physical parameters that can be referenced by individual pipes to ensure consistency and reduce redundancy.
  - `defaultK` (float) - Correction factor modifying the thermal loss calculation in a pipe. Ideally, this value should be **1.0**, meaning no correction is needed. However, since the model cannot account for all real-world properties, this factor is used to adjust heat loss based on measured data. The system learns this value over time to better reflect real pipeline characteristics.
  - `defaultPipeThermalConductivity` (float) - Default pipe material conductivity
  - `defaultInsulationThermalConductivity` (float) - Default insulation conductivity
  - `defaultSoilThermalConductivity` (float) - Default soil conductivity
  - `defaultConvectionCoefficient` (float) - Default convection coefficient
  - `defaultSurfaceEmissivity` (float) - Default surface emissivity
  - `defaultInsulationThickness` (float) - Default insulation thickness in meters

**Example JSON:**
```json
{
  "NumParams": {
    "Pre-insulated": 0.035,
    "Standard": 0.05
  },
  "defaultK": 1.0,
  "defaultPipeThermalConductivity": 50.0,
  "defaultInsulationThermalConductivity": 0.04,
  "defaultSoilThermalConductivity": 1.5,
  "defaultConvectionCoefficient": 20,
  "defaultSurfaceEmissivity": 0.9,
  "defaultInsulationThickness": 0.1
}
```

- **Hot Section Correction Factors**: Each section has a coefficient that adjusts heat loss calculations. The default value for this coefficient is **1.0**.

**Example JSON:**
```json
{
  "hotSectionCorrectionK": {
    "S1": 1.02,
    "S2": 1.05
  }
}
```

## Relationship Overview
- **Prosumers (Producers, Consumers, and Accumulators) are connected through Nodes and Pipe Sections**.
- **Nodes serve as branching points, allowing multiple Pipe Sections to interconnect**.
- **Pipe Sections reference individual Pipes with specific attributes**.
- **Thermal and hydraulic properties of the system are governed by Pipe Defaults and Correction Factors**.

This structure ensures a modular and flexible representation of the hot water distribution network while allowing easy adaptation for different implementations.
