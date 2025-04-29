 **`ConfigurationEdit.vue`** component is responsible for allowing users to edit and manage the application configuration file through a browser interface. It provides functionality for loading, editing, validating, and saving the configuration file (in JSON format), and uses a password-based mechanism for access control.

---

## Features

- **Configuration Display**: Loads and shows the current configuration file inside a textarea for editing.
- **Config Load**: Loads the current config from the backend after password verification.
- **Config Save**: Sends updated config to the backend, with validation to prevent saving broken structures.
- **Password Protection**: Users must enter a valid password before loading or saving the configuration.
- **Validation Logic**: Checks structure and types of key fields such as applications, buttons, receivers, levels, and system thresholds.

---

## Logic Overview

### **Data**
- **password**: The password used to authorize config loading and saving. It is defined in the docker-compose file.
- **configContent**: A string containing the full JSON representation of the config file.

### **Methods**
- **goToActiveSessions**: Navigates to the `/activesessions` page using Vue Router.
- **saveConfig**: Triggers config validation. If successful, emits a `save_config` event over WebSocket with the password and current content.
- **loadConfig**: Sends the password via the `load_config` WebSocket event to request the current config from the backend.
- **handleError**: Displays an error message (e.g., incorrect password or server error).
- **handleSaved**: Shows a confirmation message when the config has been saved successfully.
- **handleLoaded**: Sets the `configContent` to the loaded config and displays a success message.
- **validateConfig**: Validates that the config content is a valid JSON object with:
    - An `applications` object
    - Each `application` has valid `controlButtons`, `receivers`, and `levels` arrays
    - Each `controlButton` has a `title` and a `payload.eventName`
    - Each `level` has a valid `url`, `realWidth`, and `realHeight`
    - The `min_free_memory_percentage` field is between 0 and 100

### **Lifecycle Hooks**
- **mounted**: Registers WebSocket listeners for `error`, `config_saved`, and `config_loaded`.