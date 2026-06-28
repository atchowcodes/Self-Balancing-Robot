# Self-Balancing Robot with MPU6050 and PID Control

A two-wheeled self-balancing robot built on the NodeMCU ESP8266 microcontroller, using MPU6050 IMU sensor data and a PID closed-loop control system to maintain 
upright balance.

---

## Overview

This project implements a self-balancing robot based on the inverted pendulum principle. The robot continuously reads tilt angle from the MPU6050 sensor (accelerometer + gyroscope), filters the signal using a complementary filter, and drives two NEMA 17 stepper motors using a PID controller to maintain balance.

---

## System Architecture
<img width="947" height="523" alt="image" src="https://github.com/user-attachments/assets/f94e8265-67d8-40af-9a50-5be0ac917efe" />
Designed using Fritzing. Source file available at `main/self_balance_correct.fzz`.

### Control Flow
1. MPU6050 reads raw accelerometer and gyroscope values
2. Roll/pitch angles computed from accelerometer
3. Gyroscope integrated for angular velocity → gyro angle
4. Complementary filter fuses both to get stable tilt angle
5. PID controller computes motor correction signal
6. A4988 motor drivers actuate NEMA 17 stepper motors

### Complementary Filter
currentAngle = 0.98 * (previousAngle + gyroAngle) + 0.02 * accAngle\\

Combines slow-drifting gyroscope data with noisy-but-stable accelerometer data for accurate real-time angle estimation.

### PID Controller
motorPower = Kperror + Ki∫error + Kd*(d_error/dt)\\
**Final tuned values:** Kp = 1, Ki = 40, Kd = 0.05

---

## Hardware

| Component | Function |
|-----------|----------|
| NodeMCU ESP8266 | Main microcontroller (80MHz, 4MB flash) |
| MPU6050 IMU | 3-axis accelerometer + 3-axis gyroscope |
| 2x NEMA 17 Stepper Motor | Wheel actuation (200 steps/rev, 1.8°/step) |
| 2x A4988 Motor Driver | Stepper motor control with microstepping |
| LiPo 12V Battery | Power source |
| Lm2456 Buck Converter | 12V → 5V for NodeMCU |

---

