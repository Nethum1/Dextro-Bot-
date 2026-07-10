# Dextro-Bot-

> **ESP32 WROOM-32** powered robot with PID line following, real-time color detection, gyro-assisted stabilization, encoder feedback, and live OLED status display.

---

## 📋 Table of Contents

- [Features](#-features)
- [Hardware Components](#-hardware-components)
- [Pin Mapping](#-pin-mapping)
- [Wiring Guide](#-wiring-guide)
- [OLED Display Layout](#-oled-display-layout)
- [Software Setup](#-software-setup)
- [Library Dependencies](#-library-dependencies)
- [PID Tuning Guide](#-pid-tuning-guide)
- [Color Calibration](#-color-calibration)
- [Robot State Machine](#-robot-state-machine)
- [Project Structure](#-project-structure)
- [Troubleshooting](#-troubleshooting)
- [Competition Tips](#-competition-tips)

---


## ✨ Features

| Feature | Detail |
|---|---|
| **Line Following** | 8-sensor IR array with weighted centroid position detection |
| **PID Control** | Tunable KP, KI, KD with anti-windup integral clamping |
| **Gyro Stabilization** | MPU6050 Z-axis yaw rate correction on straight sections |
| **Color Detection** | TCS34725 RGBC sensor with real-time classification |
| **Encoder Feedback** | Hardware interrupt-driven pulse counting on both motors |
| **OLED Display** | Live status: state, IR bitmap, position, speed bars, color, encoders |
| **Motor PWM** | ESP32 LEDC hardware PWM at 20kHz (silent operation) |
| **State Machine** | Line-follow → color-stop → lost-line → finish transitions |

---
