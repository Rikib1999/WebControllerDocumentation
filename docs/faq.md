# Frequently Asked Questions (FAQ)

## 1. What is WebController?

**WebController** is a web application designed for managing and monitoring Unity-based experiments. It allows users to track active and inactive game sessions, send commands, receive data, and view player positions on a map.

## 2. What components does WebController use?

WebController is built on a client-server architecture and consists of the following components:

- **Frontend**: Developed using Vue.js for displaying session details and providing interactive controls.
- **Backend**: A Flask application managing sessions and WebSocket communication.
- **Unity Client**: Unity game sessions that communicate with the server.
- **Proxy Server**: An NGINX reverse proxy for routing requests.
- **Dockerized Environment**: Components are containerized for easy deployment and scalability.

## 3. How do I install WebController?

### Prerequisites:
Before starting, make sure you have the following installed:

- Docker
- Docker Compose
- Git

### Steps:
1. Clone the repository:
    ```
    git clone https://github.com/cyberspace-lab/cyberframe-webcontroller.git
    cd cyberframe-webcontroller
    ```

2. Create and start Docker containers:
    ```
    docker-compose up --build
    ```
    This will build and start the application, including the Flask backend, Vue frontend, and NGINX reverse proxy.

## 4. How do I configure the application?
Configuration for each experiment is done in the config.json file located at ./vuefrontend/src/config.json.

The file allows you to define:

- Control Buttons: Custom buttons for sending commands to Unity clients.
- Data Receivers: Defines the data expected from Unity clients.
- Levels: Map and level data, including size and calibration settings.
- Memory Settings: Minimum free memory percentage before cleaning inactive sessions.

Example:
```json
{
  "applications": {
    "MoveDifferent": {
      "controlButtons": [
        {
          "title": "Set participant name",
          "requiresInput": true,
          "inputPlaceholder": "Participant",
          "payload": {
            "eventName": "participantName",
            "parameters": {}
          },
          "context": ["menu"]
        }
      ],
      "receivers": [
        {
          "currentScene": { "maxHistory": 10 },
          "location": { "maxHistory": 20 },
          "position": { "maxHistory": 10 }
        }
      ],
      "levels": [
        {
          "1": {
            "url": "https://example.com/map.png",
            "realWidth": 25.5,
            "realHeight": 26.5,
            "mapOffsetX": -0.7,
            "mapOffsetY": 3.3,
            "mapOffsetRotation": -90
          }
        }
      ]
    }
  },
  "min_free_memory_percentage": 20
}
```

## 5. How do I view active and inactive sessions?
### Active Sessions:
You can view the list of active sessions by navigating to:

```
{APPLICATION_URL}/activesessions
```
Here, you can see details about currently active game sessions, including:

- Session name
- Device ID
- Connection time
- Last data update time

### Inactive Sessions:
Inactive sessions can be viewed at:

```
{APPLICATION_URL}/inactivesessions
```
You can view historical data for inactive sessions and remove them from the server.

## 6. How do I interact with Unity game sessions?
You can send commands to Unity game sessions from the Control Panel on the active session detail page. Each button is associated with a specific command, and if the command requires input, you can enter it in the provided input field.

Commands are defined in the config.json file and include parameters, such as starting or stopping the experiment or setting participant information.

## 7. How does position tracking work?
The application can track the positions of players and objects in the game world. To enable position tracking, you need to define a position data receiver in the config.json file.

The position of players and objects is displayed on the map in real-time, with a history of past positions. The map updates automatically when the level changes.

Positions are shown as dots with arrows indicating rotation, and the path of movement is drawn by connecting the positions.

## 8. How can I test the application with fake data?
You can test the frontend and backend communication with fake data by visiting:

```
http://localhost:4000/test
```
Here, you can simulate fake sessions and send fake data to specific sessions by providing a fake experiment name and device ID.

## 9. How do I integrate Unity with WebController?
To integrate Unity with WebController, you'll need to:

Add the Unity package (which includes several scripts) to your project.
Use the WebControllerManager script to connect your Unity project to the server. This script should be attached to a GameObject in the main scene.
Use the SendData() method to send data to the server and the GenericEventListener script to listen for commands from the server.
Use the PositionSender script to send position data for objects and players.
For detailed instructions, refer to the Unity Integration section.

## 10. How can I remove inactive sessions from the server?
On the detail page of an inactive session, you'll find an option to remove the session from the server. This action is permanent and cannot be undone.