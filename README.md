# ABOUT

This repository has code for managing different controllers, reading and updating various parameters, and ensuring thread-safe access to parameters to prevent unsafe/unexpected behavior.

### JSONFileHandler
- Loads all data from `config.json`
- Eliminates hardcoded values in other files to ensure consistency across controllers

### Configurable
- Base class that provides thread safe operations for retrieiving parameter values from `JSONFileHandler` and storing in a map
- Provides thread safe functions for live parameter updates in a map
- Uses a **mutex** to prevent multiple threads from simultaneously changing values in our map 

### ControllerManager
- Inherits from `Configurable`, enabling access to parameters from `JSONFileHandler`
- Manages various controllers and selects the appropriate one based on required actions (e.g., speed vs. torque control)

### Controller
- Base class for all controller types
- All controllers must implement:
  - `step_controller()`: Retrieves the current car state and outputs specific control values (e.g., torque per wheel)
  - `get_dt_sec()`: Returns the time step between controller updates
