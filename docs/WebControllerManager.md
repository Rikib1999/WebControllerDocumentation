The **WebControllerManager** is a Unity MonoBehaviour class responsible for managing network communication between a Unity application and a server using **Socket.IO**. It handles:

- Connection and disconnection events
- Data transmission to the server
- Reconnection logic
- Heartbeat implementation to maintain connection
- Singleton pattern to ensure a single instance throughout the application

---

### **Fields and Properties**

| Type                         | Name                    | Description                                                  |
| ---------------------------- | ----------------------- | ------------------------------------------------------------ |
| `string`                     | `serverURL`             | URL of the server to connect to.                             |
| `string`                     | `port`                  | Port number for the server connection.                       |
| `bool`                       | `useLocalhost`          | If true, overrides `serverURL` to use `http://localhost`.    |
| `string`                     | `sessionName`           | Name of the session for identification.                      |
| `bool`                       | `debug`                 | Enables debug logs if set to true.                           |
| `float`                      | `reconnectionTime`      | Time interval between reconnection attempts (in seconds).    |
| `static string`              | `deviceId`              | Unique identifier for the device.                            |
| `static SocketIOUnity`       | `Socket`                | The Socket.IO client instance.                               |
| `static eConnectionState`    | `ConnectionState`       | Current state of the network connection.                     |
| `static Action`              | `OnConnected`           | Event invoked when connected.                                |
| `static Action`              | `OnDisconnected`        | Event invoked when disconnected.                             |
| `static Action`              | `OnConnecting`          | Event invoked when connecting.                               |
| `static Action`              | `OnReconnecting`        | Event invoked when reconnecting.                             |
| `static WebControllerManager`| `Instance`              | Singleton instance of the manager.                           |

---

### **Enumerations**
**eConnectionState**: Represents the state of the WebSocket connection.

- **Connected**: The connection is active.
- **Disconnected**: The connection is inactive.
- **Connecting**: The connection is in the process of being established.
- **Reconnecting**: The connection is in the process of being re-established after a failure.

---

### **Methods**
- **Init**: Initializes the WebSocket connection and starts the connection attempt.
- **Connect**: Attempts to establish the WebSocket connection if disconnected.
- **Update**: Periodically checks if the connection is alive by sending ping messages and handling disconnections.
- **Register**: Sends registration data (device ID and session name) to the server to identify the device.
- **SendData**: Sends custom data to the server with a specified key and value.
- **SendPositionData**: Sends position data to the server.
- **SetContext**: Sends context data to the server (serialized as a JSON array).
- **SendDataInternal**: The internal method to send data to the server (called by the other `SendData` methods).

---

### **Socket Events**
- **OnConnected**: Triggered when a successful connection is made to the server.
- **OnDisconnected**: Triggered when the WebSocket connection is lost.
- **pong**: Listens for the "pong" event from the server to track the connection health.
- **error**: Logs errors encountered during socket communication.
- **registered**: Logs successful registration with the server.

---

### **Ping/Pong Mechanism**
- The class sends a **ping** message to the server every 2 seconds.
- It expects a **pong** message from the server within 6 seconds. If the pong message is not received, the connection state is set to **Disconnected**, and a reconnection attempt is initiated.

---

### **Lifecycle and Cleanup**
- **Awake**: Ensures that only one instance of `WebControllerManager` exists. Initializes necessary components and calls `Init`.
- **OnDestroy**: Cleans up the WebSocket connection by disconnecting and disposing of the SocketIO client.
  
---

### **Reconnection Logic**
- If the WebSocket connection is lost, the **`Update`** method monitors the time since the last **pong** message. If the server does not respond within 6 seconds, the system tries to reconnect by calling **`Connect`**.

- The **`Connect`** method attempts to reconnect by calling **`TryConnectWithDelay`** with an interval defined by `reconnectionTime`.

- If the connection is successful, the `OnConnect` method is triggered, registering the device with the server again.

---

### **Sending Data & Context**
To send data to the server, you can use the `SendData` method, which requires a `key` and a `value`. The `key` can be any string except `"position"` and `"context"`, as those are reserved for internal use. For example, to send a custom piece of data:

```csharp
WebControllerManager.SendData("key_name", "value_data");
```

To send context data, such as session-specific information, you can use the `SetContext` method. This method expects an array of strings, which will be serialized and sent as the `"context"`:

```csharp
WebControllerManager.SetContext(new string[] { "context_data1", "context_data2" });
```

---

### **Connection Events**
The `WebControllerManager` provides several events related to the WebSocket connection status. These events help you manage the connection lifecycle:

- **`OnConnecting`**: Triggered when the connection is in the process of being established.
  
  ```csharp
  WebControllerManager.OnConnecting += () => Debug.Log("Connecting to the server...");
  ```

- **`OnConnected`**: Triggered once the connection is successfully established. You can use this event to handle actions after the connection is live.
  
  ```csharp
  WebControllerManager.OnConnected += () => Debug.Log("Connected to the server!");
  ```

- **`OnDisconnected`**: Triggered when the connection is lost or the socket disconnects. Use this to handle disconnection scenarios.
  
  ```csharp
  WebControllerManager.OnDisconnected += () => Debug.Log("Disconnected from the server.");
  ```

- **`OnReconnecting`**: Triggered when the system attempts to reconnect after losing the connection. You can use this event to notify users that a reconnection is in progress.
  
  ```csharp
  WebControllerManager.OnReconnecting += () => Debug.Log("Reconnecting to the server...");
  ```

By subscribing to these events, you can keep track of the connection state and perform actions like UI updates or retry logic based on the connection status.

---

### **Additional Notes**
- The `WebControllerManager` is designed to run as a singleton, ensuring there is only one instance managing the WebSocket connection.
- The `UnityMainThreadDispatcher` component is used to ensure that certain actions happen on the Unity main thread, as Unity’s API must be accessed from the main thread.
- The `SocketIOUnity` library is used to manage WebSocket communication.