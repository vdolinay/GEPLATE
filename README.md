# GEPLATE - Software Components and Directory Structure

## Overview
GEPLATE is a platform designed to support the decentralization of district heating networks (SZT, the designation used in the Czech language, DH). It provides a software framework for collecting and providing access to relevant technological and external data, performing modeling and simulation, and offering decision support. The platform was developed as part of the research project TK04020222 within the THÉTA Program for Applied Research, Experimental Development, and Innovation. It incorporates multiple software modules, which are structured as follows:

## Research Project Details
- **Project Number:** TK04020222 
- **Project Name:** DECENTRALIZACE ZDROJŮ V SOUSTAVĚ REGIONÁLNÍHO ZÁSOBOVÁNÍ TEPLEM
- **Programme:** THETA
- **Provider:** Technologická agentura České republiky  
- **Duration:** 1.1.2022 – 31.12.2024  
- **Main Recipient:** Univerzita Tomáše Bati ve Zlíně  
- **Project Participant:** Teplárna Otrokovice a.s. 
  

## Directory Structure
```
GEPLATE/
│── data_management/           # Data collection, management, and access
│   │── data_collection/       # Modules for acquiring data from technological units
│   │── external_data/         # Modules for accessing external services (weather, OTE market data)
│   └── data_api/              # REST API for accessing stored data
│
│── simulation/                # Simulation multi-agent environment
│   │── heat_distribution/     # Heat distribution model
│   │── prosumer_models/       # Prosumer agent models (energy consumers/producers)
│   │── agent_system/          # Multi-agent control of demand/supply
│   └── checkpoints/           # Measurement and control modules
│
│── decision_support/          # Decision support tools for operation management
│   │── pricing/               # Pricing modules for energy sources
│   │── planning/              # Central decision and planning agent
│   │── power_market/          # Short-term electricity market module
│   └── heat_planner/          # Heat supply planning module
│
│── common/                    # Shared utilities and common functions
│   └── api_utils/             # REST API communication utilities
│
└── README.md                  # Project documentation
```

## Component Descriptions

### 1. Data Management (`data_management/`)
This module handles data acquisition, storage, and retrieval for the platform. It consists of:
- **`data_collection/`** - Gathers data from technological databases and direct measurements.
- **`external_data/`** - Interfaces with external services like weather forecasting (Yr.no, Aladin) and electricity market data (OTE).
- **`data_api/`** - Provides a REST API for accessing data by other components.

### 2. Simulation Environment (`simulation/`)
The simulation module models district heating networks and integrates a multi-agent system of prosumers.
- **`heat_distribution/`** - Models heat distribution through a pipeline network using plug-flow algorithms.
- **`prosumer_models/`** - Implements energy consumers and producers as agents.
- **`agent_system/`** - Controls agent interactions based on supply-demand principles.
- **`checkpoints/`** - Represents control and measurement points for real-time validation.

### 3. Decision Support Tools (`decision_support/`)
Supports planning and operational decision-making in heat networks.
- **`pricing/`** - Determines pricing strategies for decentralized energy sources.
- **`planning/`** - Implements a central decision agent optimizing energy resource utilization.
- **`power_market/`** - Monitors short-term electricity markets to optimize energy transactions.
- **`heat_planner/`** - Coordinates the scheduling of heat supply across various energy sources.

### 4. Common Utilities (`common/`)
Contains shared components, primarily utilities for API communication.
- **`api_utils/`** - Helper functions for standardized REST API interactions.

## Technical Details
- The platform supports **REST API** for communication between modules.
- Multi-agent system components are implemented in **C# and Python**.
- Distributed computing is supported through **containerization** for scalability.
