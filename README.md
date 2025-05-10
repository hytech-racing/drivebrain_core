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
 
## Initialization

### 1. `JsonFileHandler` Loads `config.json` into Memory
   - The configuration file is loaded into the `_config` object.

### 2. Controllers and Controller Manager Initialization
   - Controllers are created.
   - `ControllerManager` is created using the initialized controllers.
   - Uses `get_parameter_value<float>("param_name")` to retrieve a parameter from loaded config values (not cached in a map) and ensures values exist.

---

## Stepping a Controller Flow

### `core::ControllerOutput step_active_controller(const core::VehicleState& input)`
   - This function is called to step the active controller.
   - Calls `step_controller()` for the current controller.

### Retrieving Cached Parameters
   - `Configurable::ParamTypes get_cached_param("param_name")`
     - Retrieves the expected parameter value if it has been previously cached in `_live_params.param_vals`.
     - If the value is **not cached**, it returns `null`.

### Handling Missing Cached Parameters
   - If `null` is returned:
     - `Configurable::ParamTypes get_live_parameter<float>("param_name")` is called.
     - The retrieved value is cached and returned.

---

## Updating Cached Parameter Values

### `Configurable::handle_live_param_update(const std::string &key, Configurable::ParamTypes param_val)`
   - Updates the cached value.
   - Notifies subscribed controllers.

### Controller Subscription
   - Controllers can subscribe to updates using `.connect()` via **boost**.


