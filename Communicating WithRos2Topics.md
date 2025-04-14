## ROS 2 Topics and Communication (Analogy: Radio and Receiver)

Nodes publish information over **topics**, which allows any number of other nodes to subscribe and access that information.

In this tutorial, you explored how data moves through a ROS 2 system using tools like `rqt_graph` and the command line.

---

### Topic Fundamentals

- A **topic** is a named bus over which nodes send and receive messages.
- **Publisher-Subscriber-Topic** architecture allows **unidirectional** data flow (ideal for simulating sensors).
- A topic has a specific **message type** (e.g., `String`, `Int32`).
- ROS 2 supports multiple programming languages including **Python** and **C++**.

---

### Example Graph (rqt_graph)

![ROS 2 Topic Example Graph](file:///home/deadlyharbor/Pictures/Screenshots/Screenshot%20from%202025-04-14%2014-56-33.png)

---

## Python Publisher

**Filename:** `robot_news_station.py`

```python
#!/usr/bin/env python3
from example_interfaces.msg import String
import rclpy
from rclpy.node import Node

class RobotNewsStation(Node):
    def __init__(self):
        super().__init__("robot_news_station")
        self.get_logger().info("Robot News Station is starting...")
        self.publisher_ = self.create_publisher(String, "robot_news", 10)
        self.timer = self.create_timer(1.0, self.timer_callback)
        self.count = 0

    def timer_callback(self):
        msg = String()
        msg.data = f"Hello, I am PYTHON robot news publisher {self.count}"
        self.publisher_.publish(msg)
        self.get_logger().info(f"Publishing: {msg.data}")
        self.count += 1

def main(args=None):
    rclpy.init(args=args)
    node = RobotNewsStation()
    rclpy.spin(node)
    rclpy.shutdown()

if __name__ == "__main__":
    main()
```

### Configuration Steps:

- Add to `package.xml`:
  ```xml
  <depend>example_interfaces</depend>
  ```
- Make file executable:
  ```bash
  chmod +x robot_news_station.py
  ```
- Add node to `setup.py` console scripts

---

## Python Subscriber

**Filename:** `robot_enthusiast.py`

```python
#!/usr/bin/env python3
import rclpy
from rclpy.node import Node
from example_interfaces.msg import String

class RobotEnthusiast(Node):
    def __init__(self):
        super().__init__("robot_enthusiast")
        self.get_logger().info("Robot News Listener is starting...")
        self.subscription = self.create_subscription(
            String, "robot_news", self.listener_callback, 10)

    def listener_callback(self, msg):
        self.get_logger().info(f"Received: {msg.data}")

def main(args=None):
    rclpy.init(args=args)
    node = RobotEnthusiast()
    rclpy.spin(node)
    rclpy.shutdown()

if __name__ == "__main__":
    main()
```

> Ensure the topic name and data type match the publisher.

---

## C++ Publisher

```cpp
#include "rclcpp/rclcpp.hpp"
#include "example_interfaces/msg/string.hpp"

class RobotNewsPublisher : public rclcpp::Node {
public:
    RobotNewsPublisher() : Node("RobotNewsTower") {
        RCLCPP_INFO(this->get_logger(), "Hello I am your C++ News node");
    }

    void start() {
        publisher_ = this->create_publisher<example_interfaces::msg::String>("robot_news", 10);
        timer_ = this->create_wall_timer(
            std::chrono::seconds(1),
            std::bind(&RobotNewsPublisher::callback, this));
    }

private:
    rclcpp::Publisher<example_interfaces::msg::String>::SharedPtr publisher_;
    rclcpp::TimerBase::SharedPtr timer_;
    int _counter = 0;

    void callback() {
        auto message = example_interfaces::msg::String();
        message.data = "Hello, Robot Enthusiast!--" + std::to_string(_counter);
        RCLCPP_INFO(this->get_logger(), "Publishing: '%s'", message.data.c_str());
        publisher_->publish(message);
        _counter++;
    }
};

int main(int argc, char **argv) {
    rclcpp::init(argc, argv);
    auto node = std::make_shared<RobotNewsPublisher>();
    node->start();
    rclcpp::spin(node);
    rclcpp::shutdown();
    return 0;
}
```

---

## C++ Subscriber

```cpp
#include "rclcpp/rclcpp.hpp"
#include "example_interfaces/msg/string.hpp"

class RobotNewsListener : public rclcpp::Node {
public:
    RobotNewsListener() : Node("robot_enthusiast2") {
        RCLCPP_INFO(this->get_logger(), "Hello I am your C++ Listener node");
    }

    void listen() {
        subscription_ = this->create_subscription<example_interfaces::msg::String>(
            "robot_news", 10,
            std::bind(&RobotNewsListener::callback, this, std::placeholders::_1));
    }

private:
    rclcpp::Subscription<example_interfaces::msg::String>::SharedPtr subscription_;
    void callback(const example_interfaces::msg::String::SharedPtr msg) {
        RCLCPP_INFO(this->get_logger(), "I heard: '%s'", msg->data.c_str());
    }
};

int main(int argc, char **argv) {
    rclcpp::init(argc, argv);
    auto node = std::make_shared<RobotNewsListener>();
    node->listen();
    rclcpp::spin(node);
    rclcpp::shutdown();
    return 0;
}
```

---

## ROS 2 CLI Tools

- List topics:
  ```bash
  ros2 topic list
  ```
- Topic info:
  ```bash
  ros2 topic info /topic_name
  ```
- Echo data:
  ```bash
  ros2 topic echo /topic_name
  ```
- Interface show:
  ```bash
  ros2 interface show example_interfaces/msg/String
  ```
- Publish manually:
  ```bash
  ros2 topic pub -r 1 /robot_news example_interfaces/msg/String "{data: 'Hello'}"
  ```
- Frequency:
  ```bash
  ros2 topic hz /topic_name
  ```
- Bandwidth:
  ```bash
  ros2 topic bw /topic_name
  ```
- Rename node/topic at runtime:
  ```bash
  ros2 run my_package my_node --ros-args -r __node:=new_name -r old_topic:=new_topic
  ```

---

## ROS 2 Bags (Data Logging)

- Record:
  ```bash
  ros2 bag record /robot_news
  ros2 bag record -o my_bag /robot_news
  ```
- Info:
  ```bash
  ros2 bag info my_bag
  ```
- Replay:
  ```bash
  ros2 bag replay my_bag
  ```

