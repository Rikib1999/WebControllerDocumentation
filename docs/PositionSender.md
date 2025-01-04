The `PositionSender.cs` script is a Unity component designed to periodically send position and rotation data of specified transforms to a server. This synchronization allows real-time tracking of objects within a Unity scene.

---

## Purpose

The main purpose of the `PositionSender` script is to:

- **Track multiple transforms** in the Unity scene.
- **Serialize their positional data** along with color information.
- **Send this data to a server** at regular intervals using the `WebControllerManager`.

---

## Fields and Properties

| Field                        | Type                         | Description                                                        |
|------------------------------|------------------------------|--------------------------------------------------------------------|
| `currentLevelID`             | `int`                        | Identifier for the current level or scene.                         |
| `intervalInSeconds`          | `float`                      | Time interval between position updates.                            |
| `transformTrackings`         | `TransformTrackingBag[]`     | Array of transforms and color data to be tracked and sent.         |

---

## Workflow

1. **Initialization**:
      - Upon starting, the script checks if `transformTrackings` is not null or empty.
      - Schedules `SendPositions` to be called repeatedly based on `intervalInSeconds`.

2. **Data Collection**:
      - In `SendPositions`, the script iterates over each `TransformTrackingBag` in `transformTrackings`.
      - Collects position (`posX`, `posZ`), rotation (`rotY`), and color data.

3. **Serialization**:
      - Creates a `PositionBag` for each transform, containing all relevant data.
      - Uses `JsonConvert.SerializeObject` to convert the array of `PositionBag` instances to a JSON string.

4. **Data Transmission**
      - Sends the JSON string to the server via `WebControllerManager.SendPositionData`.

---

## Usage Example

To use the `PositionSender` script:

1. **Attach the Script**:
      - Add the `PositionSender` component to a GameObject in your Unity scene.

2. **Configure Fields**:
      - **Current Level ID**: Set the `currentLevelID` to uniquely identify the scene or level.
      - **Interval**: Set `intervalInSeconds` to control how often positions are sent.
      - **Transforms to Track**: Populate the `transformTrackings` array with the transforms you wish to monitor.

3. **Assign Colors**:
      - For each `TransformTrackingBag`, assign colors for:
         - **Latest Position Color**
         - **Older Positions Color**
         - **Path Color**

4. **Ensure WebControllerManager is Set Up**:
      - Make sure the `WebControllerManager` is properly configured to handle data sending.

---

## Detailed Class and Struct Definitions

### PositionBag Struct

This struct encapsulates all the necessary data for a single tracked transform.

| Field                  | Type                 | Description                                            |
|------------------------|----------------------|--------------------------------------------------------|
| `levelID`              | `int`                | The current level or scene ID.                         |
| `index`                | `int`                | Index of the transform in the tracking array.          |
| `posX`                 | `float`              | X-coordinate of the transform's position.              |
| `posZ`                 | `float`              | Z-coordinate of the transform's position.              |
| `rotY`                 | `float`              | Y-axis rotation (Euler angle) of the transform.        |
| `latestPositionColor`  | `SerializableColor`  | Color for the latest position indicator.               |
| `olderPositionsColor`  | `SerializableColor`  | Color for older position indicators.                   |
| `pathColor`            | `SerializableColor`  | Color for the path visualization.                      |

### SerializableColor Struct

Converts a Unity `Color` into a format suitable for JSON serialization.

| Field  | Type    | Description                     |
|--------|---------|---------------------------------|
| `r`    | `float` | Red component (0-255).          |
| `g`    | `float` | Green component (0-255).        |
| `b`    | `float` | Blue component (0-255).         |
| `a`    | `float` | Alpha component (0-1).          |

- **Constructor Logic**: Multiplies RGB values by 255 to convert from Unity's 0-1 range to 0-255.

### TransformTrackingBag Class

Holds reference to a transform and associated color settings.

| Field                   | Type        | Description                                    |
|-------------------------|-------------|------------------------------------------------|
| `transform`             | `Transform` | The transform to track.                        |
| `latestPositionColor`   | `Color`     | Color for the latest position marker.          |
| `olderPositionsColor`   | `Color`     | Color for the older positions markers.         |
| `pathColor`             | `Color`     | Color for the path drawn between positions.    |