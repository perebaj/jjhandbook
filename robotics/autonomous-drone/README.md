# Autonomous Drone Missions (Nectar SDK + Linux Foundation Stack)

Replicating and contributing to autonomous drone missions using [Black Bee Drones' Nectar SDK](https://black-bee-drones.github.io/nectar-sdk/) ([GitHub](https://github.com/Black-Bee-Drones/nectar-sdk), Apache 2.0), while standardizing the surrounding stack on **Linux Foundation** projects.

Roughly 80% of autonomous drone work (mission logic, computer vision, ROS 2 messaging, telemetry and failure handling) runs on a laptop in simulation using the same code that flies on real hardware. Starting here avoids blocking learning on a full hardware bench and keeps immature code off spinning props.

## Stack preference (Linux Foundation first)

| Layer | Project | Foundation | Role |
|---|---|---|---|
| Flight firmware | **PX4 Autopilot** | Dronecode / LF | Primary flight stack (BSD license, native ROS 2 via `uXRCE-DDS`, supported by Nectar SDK) |
| Flight controller hardware | **Pixhawk** | Dronecode / LF | Open hardware standard for PX4 boards when moving to physical flight |
| Protocol | **MAVLink** | Dronecode / LF | Lightweight messaging between flight controller, companion computer and ground station |
| Programmatic SDK | **MAVSDK** | Dronecode / LF | C++/Python/Go APIs to command PX4 and read telemetry without ROS 2 in simpler projects |
| Ground Control Station (GCS) | **QGroundControl** | Dronecode / LF | Cross-platform mission planning, calibration and live telemetry |
| Embedded RTOS | **Zephyr RTOS** | Linux Foundation | Preferred RTOS for custom microcontroller firmware and peripherals |
| Observability | **OpenTelemetry** | CNCF / LF | Standardized metrics, traces and logs for mission health and vision-pipeline latency |

**Non-LF complements:** **ROS 2** and **Gazebo** are governed by the Open Source Robotics Alliance (OSRF), not the Linux Foundation, but they are PX4's official pair for autonomy and simulation. **Foxglove Studio** and **PlotJuggler** handle live topic and rosbag inspection. Later, **MuJoCo** (classical control/dynamics) and **NVIDIA Isaac Sim / Isaac Lab** (GPU-parallel RL) cover control and robot-learning courses.

## What can be tested without vs. with hardware

The simulator boundary sits between layers 3 and 4:

```text
1. Mission & state machine  (takeoff, search, land, failsafe)   ──► 100% without drone (PC + Docker)
2. Perception & AI          (ArUco, line detection, YOLO)       ──► ~80% without drone (Gazebo + webcam/rosbag)
3. Messaging & telemetry    (ROS 2, MAVLink, OpenTelemetry)     ──► 100% without drone (identical to real flight)
──────────────────────────── SIMULATOR BOUNDARY ────────────────────────────
4. Flight controller        (PX4: EKF, flight modes, PID)       ──► Logic in SITL; PID tuning on real frame only
5. Physical hardware        (IMU, ESC, motors, LiPo, RF, frame) ──► Bench (props off) + real flight
```

- **Mission & state machine (100% without hardware):** decides what the drone does and how it recovers from failures (lost target, low battery, dropped link). Runs the exact same ROS 2 code in simulation and in flight.
- **Perception & AI (~80% without hardware):** Gazebo validates the full camera → detector (ArUco markers, line detection, YOLO) → velocity command loop. What simulation misses: propeller-induced motion blur, harsh outdoor light/shadows and real FPS on an embedded board. Printed ArUco markers on a laptop webcam and offline rosbag replay close part of this gap before buying a drone.
- **Messaging & observability (100% without hardware):** ROS 2 topics, MAVLink links, reconnects, and OpenTelemetry spans/metrics (e.g., frame capture → inference → command latency) behave identically in SITL and on real hardware.
- **Flight controller (logic in SITL, tuning on real hardware):** PX4 SITL runs the real autopilot firmware as a Linux process, validating flight modes, geofence, failsafes and EKF integration. PID tuning depends on the real frame's mass, stiffness and inertia.
- **Physical hardware (bench + flight only):** IMU noise from motor vibration, ESC heat, LiPo voltage sag under load, compass EMI, wind and ground effect cannot be fully validated in simulation.

**Isolation rule:** never debug mission software and flight hardware at the same time. Prove the mission code in SITL and prove stable manual flight separately before combining them.

## Development and learning phases

### Phase 1 — Zero hardware (PC + Docker)

- Boot Nectar SDK using prebuilt [Docker Hub](https://hub.docker.com/u/blackbeedrones) images with **PX4 SITL**, **Gazebo** and **QGroundControl**.
- Complete the "Get started" flight in Gazebo, then run the vision examples (ArUco, line detection) and YOLO AI examples.
- Reproduce a full published competition mission (IMAV 2025, SAE-2026, IMAV 2023, CBR 2025 or SAE Eletroquad 2025) and test failure injection (lost marker, artificial camera delay, dropped link).
- Exercise real lighting and latency using a laptop webcam with printed ArUco markers or real flight rosbags.
- **Contribution angle (Prof. Bruno):** open an issue, bug fix or example on `Black-Bee-Drones/nectar-sdk` before reaching out — instrumenting missions with **OpenTelemetry** (flight metrics, traces, vision-pipeline latency) builds directly on existing observability experience and matches distributed systems / IoT research.

### Phase 2 — Desk hardware, no flying

- **Flight controller over USB only (HITL):** run real PX4 firmware on a board fed simulated sensors from Gazebo to validate serial/MAVLink links and CPU load safely.
- **Companion computer + camera on the desk:** measure actual YOLO FPS, memory usage and end-to-end latency on target compute.
- **Crazyflie (optional indoor step):** ~100 g indoor drone supported by Nectar SDK — gives a safe first taste of sim-to-real issues (short battery, drift, radio) without an outdoor field or high-current LiPos.

### Phase 3 — Real drone

Connects to Project 6 in [hardware-projects-2027.md](../hardware-projects-2027.md):

- Bench checks with **props off**: motor direction, radio response, failsafe and live telemetry.
- **Tethered** tests with props on before free flight.
- Stable manual flight first, then enable the autonomous missions already validated in Phase 1.

## Glossary

- **ROS 2:** pub/sub messaging middleware connecting robot software modules (camera, vision, control, telemetry).
- **SITL (Software-in-the-Loop):** the real autopilot firmware compiled to run as a normal process on a PC coupled to a physics simulator.
- **HITL (Hardware-in-the-Loop):** real autopilot firmware running on a physical flight controller fed simulated sensors over USB.
- **Gazebo:** 3D simulator modeling gravity, thrust, collisions and virtual cameras/sensors.
- **MAVLink:** lightweight command and telemetry protocol used by PX4 and ArduPilot.
- **IMU:** inertial measurement unit (accelerometer + gyroscope) measuring rotation and acceleration.
- **EKF:** extended Kalman filter; fuses noisy sensors (IMU, GPS, barometer, vision) into a single position/attitude estimate.
- **ESC:** electronic speed controller driving each brushless motor.
- **rosbag:** recorded ROS topics saved to disk for deterministic offline replay.
- **Sim-to-real gap:** behavioral difference between an idealized simulator and the physical world (vibration, wind, light, electrical noise).
