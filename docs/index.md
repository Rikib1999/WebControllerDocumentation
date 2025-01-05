This application allows you to monitor and interact with Unity game sessions in real-time via a web interface. You can view active and inactive sessions, send commands, and visualize player positions on a map. The WebController is a client-server application designed to enable real-time communication between Unity applications (clients) and a Vue.js frontend via a Flask backend using WebSockets (Socket.IO). The application is containerized using Docker for ease of deployment.

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