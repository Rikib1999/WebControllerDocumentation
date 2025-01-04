Flask-based server in **`app.py`** provides real-time communication for managing and monitoring device sessions using WebSockets (via Flask-SocketIO). It supports both active and inactive sessions, session commands, and memory management. The server also integrates with a front-end Vue.js application for real-time updates of session statuses.

---

## Features
- **Session Management**: Tracks both active and inactive sessions for devices, allowing for session registration, disconnection, and detailed information retrieval.
- **Real-time Communication**: Uses WebSockets for bidirectional communication between the server and clients (Vue.js frontend).
- **Memory Management**: Cleans up inactive sessions when the server detects low available memory.
- **Data Handling**: Allows real-time updates and manipulation of session data via commands and key-value pairs.
- **Persistence**: Saves inactive sessions to disk when they are no longer active, ensuring session data is not lost.

---

## Logic Overview

### **Routes**
- **`/`**: The root route, returns a "Server is running" message.
- **`/test`**: A test interface for sending commands to Unity.

### **Global Variables**
- **`sessions`**: A dictionary tracking all active sessions, indexed by device ID.
- **`vue_sessions`**: A set tracking the session IDs of connected Vue.js clients.
- **`max_history_cache`**: A dictionary caching the maximum history size for session data keys.
- **`config`**: Configuration data loaded from a `config.json` file.

### **Session Dictionary Structure**

Each session in the `sessions` dictionary follows this structure:

```python
{
    'session_name': str,     # Name of the session/application
    'start_time': float,     # Timestamp when the session started
    'last_ping': float,      # Timestamp of the last client data update
    'data': dict,            # Dictionary to store data updates
    'sid': str,              # Socket.IO session ID
    'is_connected': bool     # Connection status
}
```

### **Socket.IO Events**

- **`ping`**: Sends a ping to the client and waits for a response (`pong`).
- **`connect`**: Logs the client’s connection with the server.
- **`disconnect`**: Handles client disconnections and removes the session if it was registered.
- **`register_vue`**: Registers the Vue.js client by adding its session ID (`sid`) to a global set.
- **`register`**: Registers a new device session or reconnects an existing device session. It assigns the device a unique `sid` and sets the session as active.
- **`send_command`**: Sends a command to the device with parameters.
- **`update_data`**: Updates a session's data and ensures the data history is kept within the configured size.
- **`get_active_sessions`**: Fetches all active sessions and sends them to the client.
- **`get_inactive_sessions`**: Fetches all inactive sessions and sends them to the client.
- **`get_active_session`**: Fetches the session of a device that is active.
- **`get_inactive_session`**: Fetches the session of a device that is inactive.
- **`delete_session`**: Deletes a session and its corresponding data from disk.
- **`session_data_key_update`**: Sends updates to the frontend when session data changes.

### **Session Management**
- **Active Sessions**: Devices that are currently connected and sending regular pings are considered active. These sessions are tracked in memory.
- **Inactive Sessions**: Devices that are disconnected but whose session data is still available are considered inactive. These sessions are stored in disk files for persistence (`INACTIVE_SESSIONS_DIR`).
- **Session Registration**: When a device connects, it registers with the server by sending a `register` event. If a session exists on disk, it's restored. If not, a new session is created.
- **Session Commands**: Device data can be updated using the `update_data` event, with the server managing the data's history size based on configuration.

### **Persistence Mechanism**
Inactive sessions are saved to disk to persist their data when the device disconnects. These sessions are saved under the directory `INACTIVE_SESSIONS_DIR`. When a device reconnects, its session is loaded from disk if it exists.

### **Memory Management**
- **Memory Monitoring**: The server monitors available memory using the `psutil` library. If the available memory falls below a configured threshold, it will clean up inactive sessions to free up memory.
- **Inactive Session Cleanup**: The `cleanup_inactive_sessions` function sorts inactive sessions by the last ping time and removes them to ensure the server has sufficient free memory.

### **Session Storage Functions**
- **`save_session_to_disk(device_id, session)`**: Saves a session to disk (in `INACTIVE_SESSIONS_DIR`).
- **`load_session_from_disk(device_id)`**: Loads a session from disk if it exists.
- **`get_active_sessions()`**: Returns all active sessions.
- **`get_inactive_sessions()`**: Returns all inactive sessions and loads any inactive session data from disk.
- **`emit_sessions_update()`**: Emits the updated list of active and inactive sessions to all Vue clients.