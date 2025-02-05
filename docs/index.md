This application allows you to monitor and interact with Unity game sessions in real-time via a web interface. You can view active and inactive sessions, send commands, and visualize player positions on a map. The WebController is a client-server application designed to enable real-time communication between Unity applications (clients) and a Vue.js frontend via a Flask backend using WebSockets (Socket.IO). The application is containerized using Docker for ease of deployment.

This solution consists of a web application and a Unity package. The web application includes a frontend and a server. In the web application, the user can see a list of all currently connected Unity sessions as well as a list of past, inactive sessions. Upon selecting a session from the list, its details are displayed. The session detail view shows basic information about the session. The detail view includes a control panel consisting of buttons that represent commands sent to the Unity session. These buttons may also have text fields for command input. The session detail also displays data received from the Unity session in real-time, organized into lists based on data types. Additionally, the detail view shows a map of the currently running level in the Unity session, displaying the positions, rotations, and paths of the player and other objects in the scene in real-time. A Unity session can be controlled indefinitely using only the control panel, within the scope of the Unity package implementation in the given project. The session details can be saved to a file in JSON format.

The Unity package in the session automatically connects to the server when the project is launched and immediately appears in the list of active sessions. Implementing the Unity package into a project is simple and does not require any changes or adjustments to the project’s logic. The package consists of 5 scripts. The main script is a manager script where the connection parameters to the server are configured. Receiving events from the server is handled by a separate script, where the names of the received commands are defined along with their corresponding reactions in the form of Unity events. Sending data about the positions of the player and other objects is also managed by a separate script, where it is only necessary to pass references to the respective objects and the player. Sending custom data to the server is done via a static method through code, with data always sent as a key-value pair of string type.

For more information on the functionality and control of the application, please refer to the section [Manual](manual.md).

---

## Github repositories

- **WebController**: <https://github.com/cyberspace-lab/cyberframe-webcontroller>
- **Unity package**: <https://github.com/cyberspace-lab/cyberframe-webcontroller-unitypackage>

---

## Dependencies

- **SocketIOUnity** by itisnajim: <https://github.com/itisnajim/SocketIOUnity>

---

## Application Architecture

The Web Controller Application is built on a **client-server architecture**:

- **Frontend**: Implemented using **Vue.js** framework.
- **Backend**: Utilizes **Flask**, a Python web framework.
- **Communication**: Real-time, two-way communication using **WebSockets** via **Socket.IO**.
- **Reverse Proxy**: **Nginx** is used to proxy requests between clients and the server.
- **Unity Integration**: Unity game sessions act as clients communicating with the server.
- **Containerization**: The application is dockerized for easy deployment.

---

## Main Features

- **Active and Inactive Session Management**  
  Easy tracking of game sessions with separate lists for active and inactive ones. Includes detailed logs like experiment name, device ID, timestamps, and options to save session data.

- **Live Map View**  
  Displays player and object positions on a map in real-time, showing movement paths and directions. Supports switching maps for different levels.

- **Customizable Control Panel**  
  Context-sensitive control panel to manage experiments, offering actions like starting, stopping, resetting, or moving players. Includes dynamic parameter inputs for flexible command execution.

- **Data Display and Organization**  
  Real-time data from sessions is shown clearly and grouped by type, with limits on how much past data is stored.

- **Automatic Session Management**  
  Automated saving of sessions on disconnection and memory cleanup to maintain system performance. Sessions are stored on disk and can be exported as JSON.

- **Flexible Configuration Options**  
  Fully configurable via JSON file, allowing easy customization of button actions, data types, level maps, and memory thresholds without additional coding.

- **Multi-Client and Experiment Support**  
  Supports multiple simultaneous sessions, diverse experiment types, and multiple frontend instances for scalable use cases.

- **Connection State Monitoring**  
  Tracks connection events like reconnecting or disconnecting, so experiments can react or notify users if needed.

---

## How does it work

The frontend of the web application is implemented in Vue.js. Server is implemented with Flask. Unity sessions with the implemented package and Vue frontends act as clients for the Flask server. Server and clients are communicating via websockets. Server is updating all clients with new data but only one specific Unity client to which a command was sent, this means that different clients can have identical commands but sending it to one of them wont affect others.

Web application needs to be configured with the configuration file. In this file is a list of different Unity experiments the application can communicate with. Experiment is defined by its name, command buttons, receivers and maps. Command button is defined by the name of the button shown in the control panel, name of the command and its static or dynamic parameters. Static parameters are defined in the config. For a dynamic parameter that will be taken as an input from a textbox, button should be configured accordingly. Receivers are names of the data types or categories that could be received from a Unity session and shown in the session detail. The maps section of the configuration defines pictures of different levels with their dimensions and IDs.

Frontend will dynamically create buttons and textboxes in the control panel of the session detail page based on the configuration. Categorized lists of the received data will also be created along with the correct map picture rendered. 

Server stores all the data in a dictionary like structure. It has a dictionary of all Unity sessions where the sessions device ID is the key. This dictionary stores all received data and additional inforamtion about a session. Server is automatically managing this dictionary and deleting older records (number of history records are defined in the configuration file).

When the user click on a command button in the control panel, server send the given command with its parameters to the Unity client which will handle it by a script named GenericEventListener. Unity is sending data to the server with the ID of the sessions device, name of the data type sent is the key and the data is the value. Server will receive this data, update its dictionary of the sessions data and propagate this update to all the frontend clients.