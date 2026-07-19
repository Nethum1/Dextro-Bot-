# Dextro-Bot

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

### Additional Components Required

| Component | Value | Qty | Purpose |
|---|---|---|---|
| Resistor | 4.7kΩ | 2 | I2C pull-ups (SDA + SCL) |
| Resistor | 10kΩ | 8 | IR sensor output pull-downs |
| Capacitor (electrolytic) | 100µF | 1 | ESP32 VIN decoupling |
| Capacitor (ceramic) | 100nF | 2 | 3.3V rail high-freq bypass |
| Diode | 1N4007 | 1 | Reverse polarity protection on LiPo |
| Slide Switch | — | 1 | Main power on/off |
| Connector | XT30 or XT60 | 1 | LiPo battery connector |
| USB-TTL Adapter | CH340 / CP2102 | 1 | Programming (if no onboard USB) |

---

## 📌 Pin Mapping

### Motor Driver (L298N)

| L298N Pin | ESP32 GPIO | Function |
|---|---|---|
| IN1 | GPIO25 | Left motor forward |
| IN2 | GPIO26 | Left motor backward |
| IN3 | GPIO27 | Right motor forward |
| IN4 | GPIO14 | Right motor backward |
| ENA | GPIO32 | Left speed PWM |
| ENB | GPIO33 | Right speed PWM |
| VSS | 5V rail | Logic supply |
| VS | 7.4V direct | Motor supply |
| GND | Common GND | Ground |

### 8-bit IR Sensor Array

| Sensor | ESP32 GPIO | Position |
|---|---|---|
| S1 | GPIO13 | Far left |
| S2 | GPIO12 | Left |
| S3 | GPIO4 ⚠ | Left-center |
| S4 | GPIO2 | Center-left |
| S5 | GPIO15 | Center-right |
| S6 | GPIO0 ⚠ | Right-center |
| S7 | GPIO16 | Right |
| S8 | GPIO17 | Far right |

> ⚠ **GPIO0** is also the ESP32 boot pin. A 10kΩ pull-up to 3.3V on GPIO0 is **required** so the ESP32 doesn't enter bootloader mode if the IR line is active during power-on.
>
> ⚠ **GPIO4** is shared with OLED RST. If OLED RST is wired to GPIO4, tie OLED RST to 3.3V via 10kΩ instead and set `OLED_RST = -1` in the code.
