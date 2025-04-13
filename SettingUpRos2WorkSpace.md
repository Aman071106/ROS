## 9️⃣ ROS2 Workspace Setup

### 📂 Create and Build Workspace

```bash
mkdir -p ros2_ws/src
cd ros2_ws/src
cd ..
colcon build  # If it fails, run: sudo apt install ros-dev-tools -y
```

### 📁 Source the Workspace

```bash
cd install
ls  # You should see setup.bash file
```

To use the workspace, source it:
```bash
source install/setup.bash
```

### 🛠️ Add to .bashrc for Persistence

```bash
gedit ~/.bashrc
```

Add the following line:
```bash
source ~/ros2_ws/install/setup.bash
```

Save and reopen terminal to activate workspace automatically.

