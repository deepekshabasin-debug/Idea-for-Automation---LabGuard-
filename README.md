# SMART LabGuard: Smart Laboratory Monitoring and Protection System 🛡️⚡

### 🏆 Smart India Hackathon (SIH) 2026 - Internal Hackathon
**Developed by Team TECHTRONIX**  
*An IoT-driven, sub-millisecond physical circuit protection and predictive maintenance network designed for academic and industrial engineering laboratories.*

---

## 📝 Project Overview

In traditional engineering labs, accidental electrical faults and wiring mistakes by students frequently result in permanent damage to expensive bench equipment (such as **₹8,000+ bench power supplies**), leading to high maintenance costs, safety hazards, and lab downtime. 

**SMART LabGuard** solves this by introducing a localized, intelligent safety protection node directly at the bench level, paired with a central wireless monitoring gateway for lab in-charges. It actively monitors, protects, alerts, and predicts equipment health on a modular, cost-effective budget (**₹1,200 to ₹2,000 per node**), paying for itself on day one.

---

## 🏗️ Three-Stage System Architecture

SMART LabGuard is built on a highly reliable, three-stage pipeline that separates critical physical hardware protection from software network reporting:

### 1. Stage 1: The Hardware Protection Layer ("The Shield")
* **Hardware-Level Sensing:** Uses the **Texas Instruments INA219 IC** to measure bus voltage, current, and power over **I2C (SDA and SCL pins)**. It also uses analog thermistors to monitor temperature in real-time.
* **Sub-Millisecond Isolation:** Utilizes ultra-fast semiconductor-based **Solid-State Switches and Electronic Fuses (eFuses)** connected in series between the power supply and the student's breadboard.
* **Network-Independent Safety:** Power disconnects physically in **sub-millisecond speeds** to prevent equipment burnout. Because the isolation circuit is hardwired into the local sensor control loop, **safety protection functions perfectly even if Wi-Fi, the central server, or the local microcontroller crashes**.

### 2. Stage 2: The Processing & Telemetry Layer ("The Brain")
* **Local Microcontroller Hub:** Powered by an **Espressif Systems ESP32 microcontroller** embedded inside each desk's smart protection unit.
* **Local Feedback Loop:** The ESP32 processes real-time sensor measurements against predefined safety thresholds and updates a **Bench-Level OLED/LCD Display** (showing current status, voltage, current, power, temperature, active limits, and operating hours).
* **Wireless Telemetry Dispatch:** Every **100 ms**, the ESP32 packetizes the real-time telemetry metrics and fault status, transmitting them wirelessly over **Wi-Fi or an ESP-Mesh network** to the central gateway.

### 3. Stage 3: The Central Monitoring & Software Layer
* **Central Server Ingestion:** A centralized server/gateway aggregates incoming wireless telemetry data from up to hundreds of active benches simultaneously.
* **Live Faculty Dashboard:** Features a web-based dashboard that provides a grid layout of all benches (such as B01 to B24), tracking total lab power consumption, active faults, and alert banners (e.g., *"Bench 04: Over-current detected. Power isolated"*).
* **Automated Fault Logging:** Persistent database logging tracks the frequency and parameters of over-current, over-voltage, and temperature warnings over operating hours to streamline technical troubleshooting.
* **Predictive Maintenance Scoring:** Runs a software algorithm based on **NIST Asset Condition Management standards** to compute a **0-100% Equipment Health Score** for each bench, helping lab technicians proactively service equipment before a permanent breakdown occurs.

---

## ⚡ Key Features

* **Sub-Millisecond Protection:** Instant electronic shutdown prevents equipment burnout during student short-circuits.
* **Two-Level Warning Mechanism:** Automatically flags yellow "Warning" thresholds before tripping red "Critical" fault states to minimize false alarms.
* **Fearless Learning Environment:** Relieves student anxiety, encouraging hands-on, hardware-level experimentation.
* **Energy Analytics & Monitoring:** Aggregates real-time energy usage patterns for the entire laboratory.
* **Scalable & Easy Deployment:** Highly modular design allows a school to start with a single bench and scale up to the entire laboratory.

---

## 🛠️ Hardware Stack & Specifications

* **Microcontroller:** Espressif Systems ESP32 SoC (built-in Wi-Fi, Bluetooth, and ESP-Mesh support)
* **Electrical Sensors:** Texas Instruments INA219 High-Precision Current/Voltage/Power Monitor (I2C interface via SDA/SCL lines)
* **Thermal Sensors:** High-accuracy Analog Thermistors (normal operating range: **28°C to 32°C**)
* **Protection Circuitry:** Texas Instruments Solid-State Switches and Integrated Electronic Fuses (eFuses)
* **Local Interface:** 128x64 I2C OLED display / character LCD
* **Power Terminals:** High-quality safety binding posts for AC/DC input and terminal circuit output

---

## 💻 Software Stack

* **Firmware:** Written in C++/Arduino, optimized for low-latency interrupt-driven threshold testing on the ESP32.
* **Gateway Server:** Node.js / Python-based central telemetry hub.
* **Frontend Dashboard:** Real-time web panel displaying active bench grids, alert history, and power consumption charts.
* **Database:** SQLite / PostgreSQL for persistent fault logging and historical diagnostics.

---

## 📊 The Working State-Machine Algorithm

              +-------------------------+
              |  1. INITIALIZATION      |
              +------------+------------+
                           |
                           v
              +-------------------------+
              |  2. ACTIVE MONITORING   | <======================+
              +------------+------------+                        |
                           |                                     |
           +---------------+---------------+                     |
           |                               |                     |
           v [Fault Detected]              v [Safe Operation]    |
  +------------------+             +---------------+             |
  |  3. FAULT STATE  |             | Update OLED   |             |
  +--------+---------+             | & Telemetry   |             |
           |                       +-------+-------+             |
           v                               |                     |
  +------------------+                     v                     |
  | 4. HW ISOLATION  |             [Delay 100ms]                 |
  |  (Sub-ms Cutoff) |                     |                     |
  +--------+---------+                     +---------------------+
           |
           v
  +------------------+
  |  5. TELEMETRY &  |
  |  FAULT LOGGING   |
  +--------+---------+
           |
           v
  +------------------+
  |  6. PREDICTIVE   |
  |   MAINTENANCE    |
  +------------------+

---

## 🗂️ Repository Directory Structure

```text
├── firmware/
│   ├── src/
│   │   ├── main.cpp         # Main ESP32 active monitoring & network loop
│   │   ├── sensors.h        # INA219 & thermal sensor reading over I2C
│   │   └── protection.h     # High-speed eFuse and solid-state relay logic
│   └── platformio.ini       # Project build configuration
├── server/
│   ├── index.js             # Local gateway server for telemetry ingestion
│   ├── database/            # Database schemas & logging scripts
│   └── algorithms/          # Predictive maintenance scoring models
├── dashboard/
│   ├── src/                 # Live monitoring React/web UI
│   └── public/              # Visual assets and dashboards
├── docs/
│   ├── blueprints/          # System schematic diagrams & layouts
│   └── sih_presentation.pdf # Techtronix SIH 2026 Presentation
└── README.md                # This file
📜 Standards & Compliance
NIST Special Publication 800-53 Maintenance Controls (MA-6): Drives real-time operational awareness and workstation asset logging.
NIST Predictive Maintenance Strategy Document: Guides the math behind cumulative degradation logs and 0-100% equipment health scoring.
🤝 Contributing & Team
SMART LabGuard was proudly designed and developed by Team TECHTRONIX for the Smart India Hackathon 2026.
For any technical inquiries, issues, or pull requests, please open an issue in this repository.
Protecting labs, empowering students, reducing electronic waste. ⚡🛡️
