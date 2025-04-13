# Bionic-Dog-Model-2

# WaveGo Bionic Dog - Raspberry Pi + ROS2 Setup Guide

## 📦 Requirements
- Raspberry Pi 4 (4GB/8GB)
- MicroSD Card (16GB minimum)
- Raspberry Pi Camera Module
- Ubuntu Server 22.04 (64-bit)
- ROS2 Humble
- Laptop (for SSH/VNC access)

---

## 🔧 Step-by-Step Setup

### 1. Flash Ubuntu Server on SD Card
- Download [Ubuntu Server 22.04 for RPi](https://ubuntu.com/download/raspberry-pi)
- Flash using Raspberry Pi Imager / Balena Etcher

### 2. Enable SSH and Wi-Fi
Place these in the SD card’s `boot` partition:
- `ssh` file (empty, no extension)
- `network-config` file (for Wi-Fi):

```yaml
network:
  version: 2
  wifis:
    wlan0:
      dhcp4: true
      optional: true
      access-points:
        "Ezhil":
          password: "0987654321"
```

### 3. First Boot (With Monitor or SSH)
- Boot RPi and login:
  ```
  username: ubuntu
  password: ubuntu
  ```
- Change password on first login

### 4. Get IP Address of RPi
```bash
hostname -I
```

Or find via router or `ping` from laptop.

### 5. SSH from Laptop
```bash
ssh ubuntu@<your_rpi_ip>
```

If public key error:
- Edit `/etc/ssh/sshd_config`:
  ```
  PasswordAuthentication yes
  PubkeyAuthentication no
  ```
- Restart SSH:
  ```bash
  sudo systemctl restart ssh
  ```

### 6. VNC for Remote Desktop (Optional)
```bash
sudo apt install xfce4 xfce4-goodies tightvncserver -y
vncserver
```
- Use VNC Viewer on laptop (connect to `<ip>:1`)

---

## 🤖 ROS2 Installation
```bash
sudo apt update && sudo apt upgrade
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt update
sudo apt install curl gnupg lsb-release
curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key | sudo tee /usr/share/keyrings/ros-archive-keyring.gpg > /dev/null
```

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

```bash
sudo apt update
sudo apt install ros-humble-desktop python3-rosdep python3-argcomplete -y
sudo rosdep init
rosdep update
```

Add to `.bashrc`:
```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

---

## 📷 Enable Camera
```bash
sudo apt install v4l-utils
v4l2-ctl --list-devices
ls /dev/video*
```
If `/dev/video0` not found: Check cable and camera support.

---

## 🧠 Advanced Features Plan
### ✅ Navigation Stack
- SLAM / Waypoint navigation
- Use `nav2` package with LIDAR or depth camera

### ✅ Vision
- Object Detection using OpenCV / TensorFlow
- Face/gesture recognition

### ✅ AI Integration
- Use Coral TPU/Jetson Nano for real-time inference
- Audio interaction or behavior-based control

### ✅ Obstacle Avoidance
- IR/Ultrasonic sensors with ROS2 node

---

## 🚀 Final Notes
- Always monitor CPU and temp usage
- Use heat sinks and proper power supply
- ROS2 nodes should be tested incrementally

---

