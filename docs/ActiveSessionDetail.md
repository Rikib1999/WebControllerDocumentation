**`ActiveSessions.vue`** component manages the detailed view of an active session for a specific device. It displays session information, controls, real-time session updates, and provides features such as map visualization and session data export. The component communicates with the backend via WebSockets.

---

## Features

- **Session Details**: Shows session name, device ID, start time, and last ping.
- **Control Panel**: Provides dynamic buttons for sending commands to the server.
- **Real-Time Data Updates**: Updates session data and UI elements live using WebSocket events.
- **Map Visualization**: Displays positions using the `LatestPositions` component.
- **Session Data**: Allows users to view session data in key-value pairs with a toggle option to reveal all values.
- **Session Export**: Allows users to download session details in JSON format.
- **Contextual Filtering of Control Buttons**: Control buttons are filtered based on the current session's context. If the session doesn't match the button's required context, the button will not be displayed.

---

## Logic Overview

### **Props**
- `deviceId`: The unique ID of the device whose session details are being viewed.

### **Data**
- **session**: The current active session object.
- **applications**: List of available applications loaded from a configuration file.
- **sessionData**: Real-time data associated with the current session.
- **showAllValuesToggle**: Reactive object to manage visibility of extra session data values.
- **dynamicInputs**: Array of inputs linked to control buttons requiring user input.

### **Computed Properties**
- **application**: Retrieves the current application based on the session name.
- **filteredControlButtons**: Filters control buttons based on the current session context.
- **currentContext**: Parses and returns the session's context as an array.
- **parsedPositions**: Parses position data for rendering on the map.
- **filteredPositionsByLevelID**: Filters device positions based on the current level ID.
- **filteredSessionData**: Excludes position and context data for displaying the session’s key-value pairs.
- **currentLevelID**: Extracts the current level ID from parsed positions.
- **currentMapUrl**: URL of the map image for the current level.
- **realMapWidth**: Real width of the map for the current level.
- **realMapHeight**: Real height of the map for the current level.
- **shouldRenderLatestPositions**: Determines if the LatestPositions component should be rendered.
- **mapOffsetX, mapOffsetY, mapOffsetRotation**: Offset values for positioning and rotating the map.

### **Methods**
- **goToActiveSessions**: Navigates back to the active sessions list.
- **handleButtonClick**: Handles button clicks and sends commands to the server with the associated payload and user input.
- **toggleShowAllValues**: Toggles the visibility of additional values for session data keys.
- **handleUnityDisconnected**: Handles the disconnection of the Unity application, resetting session data and redirecting to a different view.
- **handleSession**: Handles receiving session data from the server and updates the component's session state.
- **handleSessionDataKeyUpdate**: Updates specific session data in real-time.
- **emitGetActiveSession**: Requests the active session details from the server.
- **saveSessionAsJson**: Saves the session data as a JSON file for download.

### **Socket Events**
- **connect**: Emits the `get_active_session` event when the socket connects.
- **active_session**: Receives active session data from the server.
- **session_data_key_update**: Updates session data with real-time changes.
- **unity_disconnected**: Handles the disconnection of the Unity application.

### **UI Elements**
- **Session Details Section**:
  - Displays session information, including session name, device ID, start time, and last ping.
  - Buttons to save session data or navigate back to active sessions.
  
- **Map Section**:
  - Displays device positions on the map, including dynamic offsets and rendering options.
  
- **Control Panel**:
  - Displays buttons for controlling the session, including input fields for commands requiring user input.

- **Session Data Section**:
  - Displays session data in key-value pairs with toggle functionality to show all values for a given key.