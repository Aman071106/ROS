
# ROS 2 Node Development Guide

## 1. `.bashrc` Configuration

```bash
source /opt/ros/jazzy/setup.bash
source ~/myros2_ws/install/setup.bash
```

These lines ensure your ROS 2 environment is sourced properly.

## 2. Auto-Completion Feature

Type `ros2` and press <kbd>Tab</kbd> twice to see available commands:

```
action         multicast
bag            node
component      param
daemon         pkg
doctor         run
...and more
```

Use this trick with other commands too.

---

## 3. Running Nodes

```bash
ros2 run <packagename> <executablename>
```

### Common Error

If a package is not found after creation, ensure this line is in `.bashrc`:

```bash
source ~/myros2_ws/install/setup.bash
```

---

## 4. Tool Help

Get help for any tool by using:

```bash
ros2 run -h  # Replace `run` with your desired tool
```

Examples:

```bash
ros2 node list
ros2 node info <nodename>
ros2 node -h
```

---

## 5. Node Naming

Avoid using the same node name for multiple nodes.

### Warning Example:

```
WARNING: Be aware that there are nodes in the graph that share an exact name...
/mycppNode
/mycppNode
```

### Rename Node at Runtime

```bash
ros2 run my_first_cpp cppnode --ros-args -r __node:=newName
```

---

## 6. Colcon Build

### Build All

```bash
colcon build
```

### Build Specific Packages

```bash
colcon build --packages-select pkg1 pkg2
```

### Cleanup Mistaken Builds

```bash
rm -r <pkgname>  # From the directory where the build happened
```

### Symlink-Install (Python Only)

```bash
colcon build --symlink-install
```

**Note:** Ensure Python files are executable (`chmod +x`) for this to work.

---

## 7. RQT and RQT Graph

Launch RQT GUI:

```bash
rqt
```

### Usage:

- Load plugins from the `Plugins` menu.
- Save layouts using `Perspectives`.

To visualize nodes and topics:

```bash
rqt_graph
```

![alt text](image.png)

---

## 8. Turtlesim Package

### Launch Simulation:

```bash
ros2 run turtlesim turtlesim_node
ros2 run turtlesim turtle_teleop_key
```

### Multiple Turtles

Use node renaming to run multiple instances.