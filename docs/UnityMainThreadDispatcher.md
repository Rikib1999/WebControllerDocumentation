The `UnityMainThreadDispatcher` class is designed to allow the execution of actions on Unity's main thread. Unity requires that many of its API calls be made on the main thread, and this class provides a way to enqueue actions that need to be executed during the Unity game loop. This script is required by the WebControllerManager.

---

### **Properties**
- **executionQueue**: A private static queue that holds `Action` objects to be executed on the main thread.

---

### **Methods**
- **Update**: This method runs every frame during Unity's game loop. It dequeues and executes all the actions that have been queued, ensuring that they are run on the main thread. 

- **Enqueue**: This static method allows other components or threads to add actions to the queue for execution on the main thread. The actions are queued in a thread-safe manner.
  - **Parameters**:
    - `action` (Action): The action to be executed on the main thread.

---

### **Thread Safety**
- The `executionQueue` is locked using a `lock` statement, ensuring that it is safe for use across multiple threads. This allows for actions to be enqueued from other threads (like background threads) while avoiding concurrency issues.