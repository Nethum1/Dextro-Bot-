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

## 🔧 Hardware Components

| # | Component | Purpose | Notes |
|---|---|---|---|
| 1 | ESP32 WROOM-32 | Main controller | 3.3V logic, dual-core |
| 2 | L298N Motor Driver | Drive 2× DC motors | Dual H-bridge, 2A/ch |
| 3 | MPU6050 | 6-axis IMU (gyro + accel) | I2C address 0x68 |
| 4 | TCS34725 | RGB color sensor | I2C address 0x29 |
| 5 | 8-bit IR Sensor Array | Line detection | TCRT5000-based, digital out |
| 6 | 2× Encoder DC Motors | Locomotion + feedback | Quadrature or single-channel |
| 7 | LM2596 Buck Converter | 7.4V → 5V regulated | Adjust pot to exactly 5.0V |
| 8 | 0.96" SPI OLED (SSD1306) | Live status display | 128×64 px, 3.3V |
| 9 | 7.4V 2S LiPo Battery | Power source | 1500–2200mAh recommended |

