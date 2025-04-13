# 🚀 Ubuntu + ROS2 Jazzy Dual Boot Setup Guide

## 📁 Table of Contents
1. [Install Ubuntu](#install-ubuntu)
2. [Partition Windows Drive](#partition-windows-drive)
3. [Set Up Dual Boot](#set-up-dual-boot)
4. [Post-Install Essentials](#post-install-essentials)
5. [Install VS Code](#install-vs-code)
6. [Install ROS2 Jazzy](#install-ros2-jazzy)
7. [Setup ROS2 Environment](#setup-ros2-environment)
8. [Test ROS2](#test-ros2)

---

## 1️⃣ Install Ubuntu
1. Download Ubuntu 24.04.2 from the [official website](https://ubuntu.com/download).
2. Use **Rufus** to flash it to a USB drive.

---

## 2️⃣ Partition Windows Drive
- Shrink your C: drive using **Disk Management** in Windows.
- Allocate free space for Ubuntu (~50GB or more recommended).

---

## 3️⃣ Set Up Dual Boot
1. Boot from the Ubuntu USB.
2. Select “Install Ubuntu alongside Windows.”
3. Choose the partitioned space.
4. Complete the installation.

---

## 4️⃣ Post-Install Essentials

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install gedit git terminator -y
```

### 🔥 Terminator Shortcuts
- **Split vertically** → `Ctrl + Shift + E`
- **Split horizontally** → `Ctrl + Shift + O`
- **Close pane** → `Ctrl + Shift + W`
- **Maximize/Minimize pane** → `Ctrl + Shift + X`

---

## 5️⃣ Install VS Code

```bash
sudo snap install code --classic
```

Launch with:
```bash
code .
```

### 📆 Recommended Extensions
- **C++**
- **Python**
- **CMake Tools**
- **ROS (by ms-iot or equivalent)**

---

## 6️⃣ Install ROS2 Jazzy

📘 Follow official instructions: [ROS2 Jazzy Install Guide](https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html)

### 🧠 Choose Your ROS2 Package
- Minimal install (no GUI tools):
  ```bash
  sudo apt install ros-jazzy-ros-base
  ```
- Full desktop install:
  ```bash
  sudo apt install ros-jazzy-desktop
  ```

---

## 7️⃣ Setup ROS2 Environment

### 🔍 Test if `ros2` command is available
If not:
```bash
source /opt/ros/jazzy/setup.bash
```

### 🛠️ Set Up Permanently (Recommended)

Edit `.bashrc`:
```bash
gedit ~/.bashrc
```

Add this at the end:
```bash
source /opt/ros/jazzy/setup.bash
```

Save and reopen your terminal:
```bash
ros2
```
︌⃣ You should now see available ROS2 commands.

---

## 8️⃣ Test ROS2

### 🗣️ Launch Demo Nodes

Open two separate terminal panes using Terminator:

1. **Listener Node**
   ```bash
   ros2 run demo_nodes_cpp listener
   ```

2. **Talker Node**
   ```bash
   ros2 run demo_nodes_cpp talker
   ```

You should see messages being published and received!
