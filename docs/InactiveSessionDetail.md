**`ActiveSessions.vue`** component provides detailed information about an inactive session associated with a specific device. It allows viewing session details, displaying a map of device positions, saving session data as a JSON file, and handling session-related actions such as deletion.

---

## Features

- **Session Details**: Displays session name, device ID, start time, last ping, and allows the user to save session data as JSON.
- **Map Visualization**: Renders a map showing device positions with dynamic offsets.
- **Session Data**: Displays session data in a key-value format with a toggle feature to show additional data values.
- **Session Deletion**: Provides a button for deleting the session.
- **Real-Time Session Updates**: Handles session data updates and transitions from inactive to active sessions.

---

## Logic Overview

### **Props**
- `deviceId`: The unique ID of the device whose inactive session details are being displayed.

### **Data**
- **session**: The current session object with details about the session state.
- **applications**: A list of available applications loaded from a configuration file.
- **sessionData**: Real-time data associated with the session.
- **showAllValuesToggle**: Reactive object controlling the visibility of extra session data values.

### **Computed Properties**
- **application**: Retrieves the application configuration associated with the current session name.
- **parsedPositions**: Parses position data for rendering on the map.
- **filteredSessionData**: Filters out position and context data from the session data to display other key-value pairs.
- **currentLevelID**: Extracts the level ID from parsed position data.
- **currentMapUrl**: URL of the map image for the current level.
- **realMapWidth**: Real width of the map for the current level.
- **realMapHeight**: Real height of the map for the current level.
- **shouldRenderLatestPositions**: Determines if the LatestPositions component should be rendered.
- **mapOffsetX**: Horizontal map offset for the current level.
- **mapOffsetY**: Vertical map offset for the current level.
- **mapOffsetRotation**: Rotation offset for the current map.

### **Methods**
- **goToInactiveSessions**: Navigates back to the inactive sessions list.
- **handleInactiveSession**: Updates session details when new inactive session data is received.
- **toggleShowAllValues**: Toggles visibility for additional values in session data.
- **handleUnityConnected**: Redirects to the active session detail page when Unity connects.
- **emitGetInactiveSession**: Emits a request to fetch inactive session data.
- **deleteSession**: Deletes the current session with confirmation.
- **saveSessionAsJson**: Saves the session data as a downloadable JSON file.

### **Lifecycle Hooks**
- **mounted**: Emits an event to fetch inactive session data after the component is mounted. Listens for incoming socket events (`inactive_session`, `unity_connected`).
- **beforeUnmount**: Cleans up socket event listeners before the component is destroyed.

### **Socket Events**
- **connect**: Triggers the request for inactive session data when the socket is connected.
- **inactive_session**: Receives inactive session data from the server.
- **unity_connected**: Handles Unity application connection and redirects to the active session detail page.

### **UI Elements**
- **Session Details Section**: Displays session information (session name, device ID, start time, last ping). Includes buttons for saving the session as JSON and navigating back to inactive sessions.

- **Session Data Section**: Displays session data in a key-value format, with a toggle to show additional values for each session data key.

- **Delete Button**: Allows the user to delete the session with confirmation.