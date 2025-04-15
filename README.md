# 🐾 WaveGo Bionic Dog – Raspberry Pi + ROS 2 Setup Guide

This project focuses on developing an intelligent robotic dog using **WaveGo Bionic Dog**, powered by **Raspberry Pi** and **ROS 2**. The goal is to integrate camera vision, AI, and autonomous movement using **ROS 2** with **Ubuntu** as the operating system.

---

## 📦 Requirements

- **Raspberry Pi 4** (4GB/8GB RAM)
- **MicroSD Card** (Minimum 16GB)
- **Raspberry Pi Camera Module**
- **Ubuntu Server 22.04** (64-bit)
- **Laptop** (for SSH or VNC access)
- **ROS 2 Humble** (to be installed later)

---

## 🔧 Setup Instructions

### 1. Flash Ubuntu Server on SD Card

- Download [Ubuntu Server 22.04 (RPi)](https://ubuntu.com/download/raspberry-pi).
- Flash the image using **Raspberry Pi Imager** or **Balena Etcher**.
- Use the **64-bit** image.

---

### 2. Enable SSH and Wi-Fi on First Boot

Before inserting the SD card into the Raspberry Pi:

- In the **boot** partition:
  - Create an empty file named `ssh` (no extension) to enable SSH access.
  - Create a file named `network-config` with the following content (replace `<YOUR_WIFI_NAME>` and `<YOUR_WIFI_PASSWORD>` with your details):

    ```yaml
    network:
      version: 2
      wifis:
        wlan0:
          dhcp4: true
          optional: true
          access-points:
            "<YOUR_WIFI_NAME>":
              password: "<YOUR_WIFI_PASSWORD>"
    ```

---

### 3. First Boot (via Monitor or SSH)

- Boot up the Raspberry Pi.
- Login with the default credentials:  
  **Username:** `ubuntu`  
  **Password:** `ubuntu`  
- You will be prompted to change the password on first login.

---

### 4. Get the IP Address

Once booted, run the following command to get the IP address:

```bash
hostname -I
```

Alternatively, check your router’s connected devices list to find the Raspberry Pi’s IP.

---

### 5. SSH from Laptop

From your laptop terminal, SSH into the Raspberry Pi:

```bash
ssh ubuntu@<your_rpi_ip>
```

If you encounter a public key error, modify the SSH configuration:

- Edit the SSH config file:
  
  ```bash
  sudo nano /etc/ssh/sshd_config
  ```
  
- Ensure these lines are set:
  
  ```conf
  PasswordAuthentication yes
  PubkeyAuthentication no
  ```
  
- Then restart the SSH service:

  ```bash
  sudo systemctl restart ssh
  ```

---

### 6. Optional – VNC for Remote Desktop

To set up a GUI environment on the Raspberry Pi:

- Install XFCE and a VNC server:
  
  ```bash
  sudo apt install xfce4 xfce4-goodies tightvncserver -y
  vncserver
  ```

- Use **VNC Viewer** on your laptop to connect using:

  ```
  <your_rpi_ip>:1
  ```

---

### 7. Enable Raspberry Pi Camera

To enable and test your camera module:

- Install the necessary utilities:
  
  ```bash
  sudo apt install v4l-utils
  ```
  
- Check the camera devices:
  
  ```bash
  v4l2-ctl --list-devices
  ls /dev/video*
  ```

If `/dev/video0` is not listed, verify the camera connection and compatibility.

---

## ✅ Steps Executed So Far (Post-Setup)

### 8. Clone the GitHub Repository

Clone the project repository to your Raspberry Pi:

```bash
git clone https://github.com/EzhilsreeJ/Bionic-Dog-Model-2.git
cd Bionic-Dog-Model-2
```

---

### 9. Set a Static IP Address (Optional)

To prevent the IP address from changing on reboot, configure a static IP via Netplan:

- Edit the Netplan configuration file:

  ```bash
  sudo nano /etc/netplan/50-cloud-init.yaml
  ```

- Replace its content with the following (update `<YOUR_WIFI_NAME>` and `<YOUR_WIFI_PASSWORD>` accordingly):

  ```yaml
  network:
    version: 2
    renderer: networkd
    wifis:
      wlan0:
        dhcp4: no
        addresses:
          - 192.168.34.19/24
        gateway4: 192.168.34.1
        nameservers:
          addresses:
            - 8.8.8.8
            - 1.1.1.1
        access-points:
          "<YOUR_WIFI_NAME>":
            password: "<YOUR_WIFI_PASSWORD>"
  ```

- Apply the configuration:

  ```bash
  sudo netplan apply
  ```

---

### 10. Enable SSH Service on Boot

Ensure SSH runs on startup:

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

---

### 11. Disable Cloud-Init to Improve Boot Speed

Reduce boot time by disabling cloud-init:

```bash
sudo touch /etc/cloud/cloud-init.disabled
```

---

## ⏭️ Next Step: Install ROS 2

You are now ready to proceed with installing **ROS 2 Humble**. Documentation updates will follow once the ROS 2 installation begins.

---

## 📂 Project Repository

> [GitHub – Bionic Dog Model 2](https://github.com/EzhilsreeJ/Bionic-Dog-Model-2.git)

---

**Maintained by:** Ezhil Sree & Team  
**Platform:** Raspberry Pi 4 + Ubuntu Server 22.04  
**Status:** 🔧 In Progress
