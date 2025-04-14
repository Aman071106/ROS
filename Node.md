# 🤖 What is a ROS2 Node?

## 🧠 Concept Overview
A **ROS2 Node** is the fundamental unit of computation in a ROS2-based robotics application. Each node is designed to perform a **single, specific task**, such as reading sensor data, controlling a motor, or processing an image. 

Think of a node as a **modular building block**—many nodes can work together in a system, forming a **ROS Graph** through which they exchange information.

---

## 🔗 Nodes in the ROS2 Graph
ROS2 represents its system architecture as a **Graph**. In this graph:
- **Nodes** are the vertices (processing entities).
- **Topics, Services, and Actions** are the edges (communication channels).

Nodes can:
- **Publish** data to topics
- **Subscribe** to topics
- **Provide or consume services**
- **Send or receive action goals**

This setup allows for **seamless and scalable communication** between multiple nodes.

---

## ⚠️ Important Considerations
- **Unique Naming**: When running multiple instances of the same node, assign them **unique names** to avoid conflicts.
- **Debugging**: Modular structure improves **debugging and testing**.
- **Performance**: Use **C++ nodes** when performance and execution speed are critical; Python nodes are more convenient for rapid development.

---

## 🌟 Benefits of ROS2 Nodes
✅ Scalable architecture  
✅ Cleaner, modular code  
✅ Easier debugging and testing  
✅ Cross-language support (Python or C++)  
✅ Encourages single-responsibility design principles

---

## 💬 Communication Example Between Two Nodes
Let's say you have two separate packages:

- `package_sensor`: Contains a node that publishes temperature data
- `package_display`: Contains a node that subscribes to and displays that data

### 👇 Example
**Sensor Node (Python):**
```python
rclpy.init()
node = rclpy.create_node('temp_publisher')
publisher = node.create_publisher(Float32, 'temperature', 10)
```

**Display Node (Python):**
```python
rclpy.init()
node = rclpy.create_node('temp_listener')
subscription = node.create_subscription(Float32, 'temperature', callback, 10)
```

These nodes communicate via the `temperature` topic.

---

## 📊 Animated Flowchart

![Robust ROS2 Node Architecture](https://github.com/user-attachments/assets/96abf1d3-3604-4514-901a-78c4d90c4f9c)


---

## 🗺️ Visual Diagram
```
+---------------------+         Publishes        +------------------+
|  Sensor Node        |  --------------------->  |   ROS Topic      |
|  (package_sensor)   |                         |   'temperature'  |
+---------------------+                         +------------------+
                                                       |
                                                       | Subscribes
                                                       v
                                               +------------------+
                                               |  Display Node    |
                                               | (package_display)|
                                               +------------------+
```

---

## 📘 Summary
A **ROS2 node** is a powerful abstraction that allows developers to split robotic behavior into manageable, reusable, and independently executable components. By designing a system of interconnected nodes, you create a **robust, scalable, and maintainable** robot architecture.

# 🚀 ROS 2 Node Templates (Python + C++)

Organized guide for writing and running ROS 2 nodes using both **Python** and **C++**, including OOP patterns, timers, and build/run instructions.

---

## 📦 Directory Layout

```
workspace/
└── src/
    └── my_package/
        ├── my_package/
        │   └── mynode.py          # Python Node
        ├── src/
        │   └── my_cpp_node.cpp    # C++ Node
        ├── CMakeLists.txt
        └── package.xml
```

---

## 🐍 Python Node Template (OOP Style)

📄 `src/my_package/my_package/mynode.py`

```python
#!/usr/bin/env python3
import rclpy
from rclpy.node import Node

class MyNode(Node):
    def __init__(self):
        super().__init__("my_first_node")
        self.get_logger().info("Hello this is my first OOP node in Python")
        self.counter = 0
        self.create_timer(0.5, self.callbackFunction)

    def callbackFunction(self):
        self.get_logger().info(f"----Calling----{self.counter}")
        self.counter += 1

def main(args=None):
    rclpy.init(args=args)
    node = MyNode()
    rclpy.spin(node)
    rclpy.shutdown()

if __name__ == "__main__":
    main()
```

🔧 Make it executable:

```bash
chmod +x mynode.py
./mynode.py
```

🛠 Add executable to `setup.py`:

```python
entry_points={
    'console_scripts': [
        "my_executable = my_package.mynode:main"
    ],
}
```

Build and run:

```bash
colcon build --packages-select my_package
source install/setup.bash
ros2 run my_package my_executable
```

---

## 💻 C++ Node Template

📄 `src/my_package/src/my_cpp_node.cpp`

```cpp
#include "rclcpp/rclcpp.hpp"

class MyNode : public rclcpp::Node {
public:
    MyNode() : Node("mycppNode") {
        RCLCPP_INFO(this->get_logger(), "Hello I am your C++ node");
        timer_ = this->create_wall_timer(
            std::chrono::seconds(1),
            std::bind(&MyNode::callback, this));
    }

private:
    int counter_ = 0;
    void callback() {
        RCLCPP_INFO(this->get_logger(), "Calling %d", counter_);
        counter_++;
    }

    rclcpp::TimerBase::SharedPtr timer_;
};

int main(int argc, char **argv) {
    rclcpp::init(argc, argv);
    auto node = std::make_shared<MyNode>();
    rclcpp::spin(node);
    rclcpp::shutdown();
    return 0;
}
```

---

## ⚙️ Add to `CMakeLists.txt`

```cmake
add_executable(my_cpp_node src/my_cpp_node.cpp)
ament_target_dependencies(my_cpp_node rclcpp)

install(TARGETS
  my_cpp_node
  DESTINATION lib/${PROJECT_NAME}
)
```

Build:

```bash
colcon build --packages-select my_package
```

Run:

```bash
source install/setup.bash
ros2 run my_package my_cpp_node
```

---

## ✅ Pro Tips

- ✅ Make file name, node name, and executable name consistent
- 💾 Always save files before building
- 🧱 Rebuild after every change using `colcon build`

![ROS2 Libraries](https://github.com/user-attachments/assets/949667cd-1fbd-4278-b66f-19c18962c25b)
