# 📦 ROS2 Package Creation Guide

## 📁 Packages in ROS2
Packages are fundamental building blocks in ROS2. They encapsulate code related to specific functionalities of your robot.

> ⚠️ All packages should be created inside the `src` folder of your workspace.

There are two main types of packages:
- **Python Packages**
- **C++ Packages**

---

## 🐍 Creating a Python Package

```bash
cd src
ros2 pkg create my_package_name --build-type ament_python --dependencies rclpy
```

### 📌 Explanation:
- `ament_python`: Build system
- `colcon`: Build tool
- `rclpy`: Python client library for ROS2

### 📁 Folder Structure
After creation, your package folder includes:
- `my_package_name/` → where you write your Python code
- `resource/` and `test/` folders
- `package.xml` → dependencies and metadata
- `setup.py` and `setup.cfg` → required to install nodes

### 🧪 Build the Package
Go to workspace root and run:
```bash
colcon build
# or build a specific package
colcon build --packages-select my_package_name
```

Once built, your package is ready to run Python nodes.

---

## 💻 Creating a C++ Package

```bash
cd src
ros2 pkg create my_package_name --build-type ament_cmake --dependencies rclcpp
```

### 📌 Explanation:
- `ament_cmake`: Build system for C++
- `rclcpp`: C++ client library for ROS2

### 📁 Folder Structure
After creation, your package folder includes:
- `include/my_package_name/` → header files
- `src/` → source files
- `CMakeLists.txt` and `package.xml`

### 🧪 Build the Package
From workspace root:
```bash
colcon build
# or build a specific package
colcon build --packages-select my_package_name
```

---

## 🧹 Cleaning Up
If you accidentally run a build in the wrong directory, remove generated folders using:
```bash
rm -r build/ install/ log/
```

Make sure to run `colcon build` from your workspace root (`ros2_ws`).

---

🎉 Your packages are now ready to host ROS2 nodes!