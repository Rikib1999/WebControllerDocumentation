The **`InactiveSessions.vue`** file is a Vue.js component for displaying inactive sessions. It provides navigation to active sessions and updates the session list in real-time via Socket.IO.

---

## Features

- **Inactive Sessions List**: Displays a list of inactive sessions with session names, device IDs, start times, and last ping times.
- **Navigation**: Allows navigation to the session details page of each inactive session and to the active sessions page.
- **No Sessions Message**: Displays a message when no inactive sessions are available.
- **Real-Time Updates**: Updates the inactive sessions list in real time when new data is received.

---

## Logic Overview

### **Data**
- **inactiveSessions**: An object that holds the inactive session data, indexed by device ID.

### **Methods**
- **goToActiveSessions**: Navigates to the active sessions page when the corresponding button is clicked.
- **handleInactiveSessionsUpdate**: Updates the `inactiveSessions` data when the 'inactive_sessions_update' event is received from the server.

### **Lifecycle Hooks**
- **mounted**: Emits an event to fetch the inactive session data when the component is mounted. Listens for the `inactive_sessions_update` event to update the session list.
- **beforeUnmount**: Removes the event listener for the `inactive_sessions_update` event before the component is destroyed.

### **UI Elements**
- **Banner**: A banner displaying the title "Inactive Sessions" and a button to navigate to the active sessions page.
- **Session List**: A list of inactive sessions, where each item displays:
  - **Session Name**: The name of the inactive session.
  - **Device ID**: The unique ID of the device.
  - **Start Time**: The start time of the session.
  - **Last Ping**: The time of the last ping received.
  - **View Detail**: A link to view detailed information about the session.
- **No Inactive Sessions Message**: A message shown if there are no inactive sessions.

### **Socket Events**
- **get_inactive_sessions**: Sent to request the inactive sessions data when the component is mounted.
- **inactive_sessions_update**: Receives updated inactive session data from the server, triggering the `handleInactiveSessionsUpdate` method to update the inactive session list.