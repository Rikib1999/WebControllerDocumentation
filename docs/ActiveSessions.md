The **`ActiveSessions.vue`** file is a Vue.js component designed to manage and display active sessions. It utilizes Socket.IO for real-time updates and offers navigation to both detailed views of active sessions and a list of inactive sessions.

---

## Features

- **Navigation**: Allows navigation to the session details page of each active session and to the inactive sessions page.
- **Active Sessions List**: Shows session details such as session name, device ID, start time, and last ping. Each session links to a detail view.
- **No Sessions Message**: Displays a message when no active sessions are available.
- **Real-Time Updates**: Updates the active sessions list in real time when new data is received.

---

## Logic Overview

### **Data**
- **activeSessions**: An object that holds the active session data, indexed by device ID.

### **Methods**
- **goToInactiveSessions**: Navigates to the inactive sessions page when the corresponding button is clicked.
- **handleActiveSessionsUpdate**: Updates the `activeSessions` data when the 'active_sessions_update' event is received from the server.

### **Lifecycle Hooks**
- **mounted**: Emits an event to fetch the active session data as soon as the component is mounted. Listens for the `active_sessions_update` event to update the session list.
- **beforeUnmount**: Removes the event listener for the `active_sessions_update` event before the component is destroyed.

### **UI Elements**
- **Banner**: A banner displaying the title "Active Sessions" and a button to navigate to the inactive sessions page.
- **Session List**: A list of active sessions, where each item displays:
  - **Session Name**: The name of the active session.
  - **Device ID**: The unique ID of the device.
  - **Start Time**: The start time of the session.
  - **Last Ping**: The time of the last ping received.
  - **View Detail**: A link to view detailed information about the session.
- **No Active Sessions Message**: A message shown if there are no active sessions.

### **Socket Events**
- **get_active_sessions**: Sent to request the active sessions data when the component is mounted.
- **active_sessions_update**: Receives updated active session data from the server, triggering the `handleActiveSessionsUpdate` method to update the active session list.