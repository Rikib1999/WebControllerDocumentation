The `WebControllerCommandParameters` class is a custom implementation of the `IReadOnlyDictionary<string, object>`, providing a read-only interface to a collection of key-value pairs where the keys are strings and the values are objects. It is designed to store command parameters received from WebController that are then processed as a parameter of a custom method, which is implemented as an action for received command.

---

### **Usage Example**

```csharp
//Custom method for processing the received data 
public void ReceiveDataFromServer(WebControllerCommandParameters parameters)
{
  // Accessing values by key
  string username = (string)parameters["username"];
  int score = (int)parameters["score"];

  // Iterating through the parameters
  foreach (var kvp in parameters)
  {
    Debug.Log($"Key: {kvp.Key}, Value: {kvp.Value}");
  }
}
```

**Important**: Dynamic input is accessible with the key ```"userInput"```.

---

This class provides an encapsulation for web controller command parameters, storing them in a read-only dictionary. It is a flexible solution for storing and passing around parameters that can be accessed by key while ensuring the integrity and immutability of the data once set.