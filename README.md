Here is a more detailed specification for the extended VMD motion file format based on the example you provided:

### VMD Motion Extended Protocol Specification

#### Overview:
The extended VMD motion file format incorporates a sequence of events, each associated with a specific frame. These events trigger various animations or manipulations in the MMD environment, such as starting animations or applying velocities to specific nodes (bones or objects). The format allows custom actions by specifying the event type, target node, and additional parameters.

---

### 1. **Event Structure**
Each event in the file consists of the following fields:

#### 1.1. **frame** (integer):
- **Description**: The specific frame at which the event is triggered.
- **Value Range**: Any non-negative integer.
  
#### 1.2. **event** (string):
- **Description**: Describes the type of event to be executed.
- **Predefined Event Types**:
  - `"custom:startGateAnimation"`: Starts a custom animation sequence for a gate or similar object.
  - `"addVel"`: Adds velocity to a specified node.
  
- **Custom Event Types**: Can be defined based on specific use cases, allowing flexibility.

#### 1.3. **node** (string, optional):
- **Description**: The name of the node (e.g., bone or object) that the event affects.
- **Optional**: This field may be omitted for events that do not target a specific node.

#### 1.4. **param** (object, optional):
- **Description**: Parameters related to the event, encapsulated as key-value pairs. These provide additional data necessary for certain events.
- **Optional**: This field is only required for events that involve additional parameters (e.g., velocity direction and magnitude).

---

### 2. **Event Parameters**
The `param` object contains various parameters based on the event type. Below is a breakdown of potential fields:

#### 2.1. **For `addVel` Event**:
- **dir** (array of 3 floats):
  - **Description**: The direction in which the velocity is applied, represented as a vector in the format `[x, y, z]`.
  - **Value Range**: Floats representing the direction vector components, typically normalized or scaled.
  
- **vel** (float):
  - **Description**: The magnitude of the velocity applied to the node.
  - **Value Range**: A positive float value, indicating the speed in the given direction.

---

### 3. **Sample JSON Structure**
```json
{
    "events":[
        {
            "frame": 1,
            "event": "custom:startGateAnimation"
        },
        {
            "frame": 92,
            "event": "addVel",
            "node": "右手首",
            "param": {
                "dir": [-1, -1, 0],
                "vel": 1000
            }
        },
        {
            "frame": 1122,
            "event": "addVel",
            "node": "左手首",
            "param": {
                "dir": [-1, -1, 0],
                "vel": 100
            }
        }
    ]
}
```

---

### 4. **Event Processing Logic**
1. **Frame Execution**: At each frame, the events scheduled for that frame are processed in order of appearance.
2. **Event Execution**:
   - If the event involves a specific node (`node` field exists), the operation is applied to the corresponding bone or object.
   - For `addVel`, the specified velocity and direction are applied.
   - For `custom` events, custom logic should be defined based on the event name.

### 5. **Extensibility**
- **Custom Events**: The format is designed to allow additional events by introducing new `event` types.
- **Future Parameters**: New parameter types can be defined in the `param` object based on the needs of new events, allowing for complex interactions.

---

This format is flexible and can be adapted as necessary depending on the features required for different MMD animations.
