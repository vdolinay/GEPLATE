# GEPLATE - Software Components and Directory Structure

## Overview
GEPLATE is a platform designed to support the decentralization of district heating (DH) networks (in the Czech context referred to as SZT).
It provides a software framework for collecting and providing access to relevant technological and external data, performing modeling and simulation,
and offering decision support. 
The platform was developed as part of the research project TK04020222 within the THÉTA Program for Applied Research, Experimental Development, and Innovation.

### Research Project Details
- **Project Number:** TK04020222 
- **Project Name:** DECENTRALIZACE ZDROJŮ V SOUSTAVĚ REGIONÁLNÍHO ZÁSOBOVÁNÍ TEPLEM
- **Programme:** THETA
- **Provider:** Technologická agentura České republiky  
- **Duration:** 1.1.2022 – 31.12.2024  
- **Main Recipient:** Univerzita Tomáše Bati ve Zlíně  
- **Project Participant:** Teplárna Otrokovice a.s. 
  

## Directory structure of the repository
As mentioned in the introduction, the platform provides a software framework structured into components based on their role within the platform.
Each component consists of modules designed for a specific role. The individual components, their modules, and additional supporting tools are located in the `components` directory. However, the contents of this directory are managed by a private repository available at [GEPLATE_components](https://github.com/vdolinay/GEPLATE_components).

```
GEPLATE/
│── components/                      # Main directory for platform components
│   │── data_management/             # Data collection, management, and access
│   │   │── data_collection/         # Modules for acquiring data from technological units
│   │   │── external_data/           # Modules for accessing external services (weather, OTE market data)
│   │   │── data_api/                # REST API for accessing stored data
│   │   └── common/                  # Shared utilities and libraries of functions applicable across this component
│   │
│   │── simulation/                  # Simulation multi-agent environment
│   │   │── heat_distribution/       # Heat distribution model
│   │   │── prosumer_models/         # Prosumer (energy consumers/producers) models and their intermediary agents
│   │   │── checkpoints/             # Module for accessing measurement data at specific locations in the district heating site
│   │   │── containerization/        # Applications encapsulating prosumer models and checkpoints with communication interfaces for platform integration
│   │   └── common/                  # Shared utilities and libraries of functions applicable across this component
│   │
│   │── decision_support/            # Decision support tools for operation management
│   │   │── coordination_agent/      # A planning and decision-making agent coordinating prosumer's intermediary agents
│   │   │── pricing/                 # Pricing modules for individual prosumers
│   │   │── power_market/            # Services for electricity planning, short-term market, and balancing
│   │   │── heat_planner/            # Module for heat demand and supply planning
│   │   └── common/                  # Shared utilities and libraries of functions applicable across this component
│   │
│   │── common/                      # Shared utilities and libraries of functions applicable across platform components
│ 
│       
└── README.md                        # Overview of the project's main structure
```

## Component Descriptions  

### 1. Data Management (`data_management/`)  
This component is responsible for acquiring, storing, and providing access to data across the platform.
It includes modules for collecting data from technological units and databases,
as well as interfaces for accessing external services such as weather forecasts (Yr.no, Aladin) and electricity market data (OTE).
A dedicated REST API ensures efficient retrieval of stored data. 
Additionally, shared utilities and function libraries within this component support data handling and integration.  

### 2. Simulation Environment (`simulation/`)  
This component provides a simulation environment for modeling district heating networks and their interactions with a multi-agent system for energy production and consumption planning. It includes a heat distribution model based on plug-flow algorithms, as well as detailed models of prosumers (energy consumers or producers) along with their intermediary agents cooperating on the efficient operation of the district heating system as a whole. Containerization is a natural part of this logic, encapsulating prosumer models and checkpoints while providing them with communication interfaces to seamlessly connect with other platform components.
An auxiliary module, called Checkpoint, handles measurement data from specific measuring (control) points in the district heating network, whether based on real sensors or predictive models. Checkpoints can represent actual measuring (control) points within the distribution network or be virtually inserted for analytical and optimization purposes.
The component also includes shared utilities and function libraries to support simulation processes.  

### 3. Decision Support Tools (`decision_support/`)  
This component provides tools for planning heat production, with a connection to the electricity market, where electricity can be both offered from cogeneration and purchased for electric boilers and other equipment.
At its core is a coordination agent responsible for collecting data, making decisions, and directing intermediary agents representing prosumers. Additional modules define pricing strategies for individual prosumers, support electricity planning with integration into short-term markets and balancing services, and facilitate heat demand and supply planning. Shared utilities and function libraries further enhance decision-making capabilities across this component.  

### 4. Common Utilities (`common/`)  
This directory contains shared utilities and function libraries applicable across all platform components, ensuring consistent communication, data processing, and integration across the system. It also includes tools for testing and troubleshooting the interdependencies between components, as well as for providing access control.

## Technical Details
- **REST API** enables communication between modules, allowing them to be interconnected, run on different machines, and implemented in various programming languages.
- **Containerization** ensures efficient operation and scalability, facilitating seamless deployment and distribution of modules across different computing resources.
- **Modules** of the components are implemented in **C# and Python**.
