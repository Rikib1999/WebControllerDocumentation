The `GenericEventListener` class is designed to listen for specific events from the WebSocket server and trigger corresponding Unity events. It integrates Unity's event system with WebSocket-based events to allow for seamless communication between the server and Unity. When a relevant event is received, it invokes a Unity event with the appropriate parameters.

## Features
- **Dynamic Event Subscription**: The class subscribes to multiple WebSocket events dynamically at runtime, based on the configuration provided in the `EventBag` array.
- **Event-Driven Unity Integration**: Upon receiving data from the WebSocket server, it invokes Unity events, passing the received data as parameters of type WebControllerCommand to any subscribed listeners.
- **Main Thread Execution**: Ensures that Unity-specific actions are executed on the main thread via the `UnityMainThreadDispatcher`, preventing issues related to multi-threading.

---

### **Properties**
- **events**: A serialized array of `EventBag` objects that define the WebSocket event names and the corresponding Unity events to trigger when those events occur.
  
---

### **Methods**
- **OnEnable**: This method is called when the object is enabled. It subscribes to each event specified in the `events` array. When an event is received from the WebSocket server, it invokes the corresponding Unity event with the parameters provided by the server.
  
- **OnDisable**: This method is called when the object is disabled. It unsubscribes from all events in the `events` array, ensuring that no further events will be received or processed after the object is disabled.
  
---

### **Nested Classes**
- **EventBag**: A private, serializable class used to store the event name (`eventName`) and the corresponding Unity event (`response`) to trigger when the event is received from the WebSocket server.
    - **eventName** (string): The name of the WebSocket event to listen for.
    - **response** (UnityEvent<WebControllerCommandParameters>): The Unity event to invoke when the WebSocket event is received, with the server response wrapped in `WebControllerCommandParameters`.

---

### **Usage**

1. **Add the Component**:
    - Attach the `GenericEventListener` script to a GameObject in your Unity scene.

2. **Configure Events**:
    - In the Inspector, expand the `Events` array.
    - For each event:
        - **`Event Name`**: Enter the name of the server event to listen for.
        - **`Response`**: Add listeners to define what should happen when the event is received.

3. **Implement Responses**:
    - Use the UnityEvent interface to assign existing component methods, to respond to the event.
    - The event response receives a `WebControllerCommandParameters` object containing the event data.
    - The methods responding to the event should have either one parameter of type WebControllerCommandParameters or no parameters.