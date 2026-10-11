# IoT-Based Energy Consumption Monitoring and Efficiency Analysis of Induction Motors

![ESP32](https://img.shields.io/badge/ESP32-IoT-blue)
![Arduino](https://img.shields.io/badge/Arduino-IDE-teal)
![Blynk](https://img.shields.io/badge/Blynk-Dashboard-green)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)

Real-time electrical parameter monitoring of a 3-phase induction motor using ESP32 and IoT cloud dashboard. AC Machines mini project, VIT Chennai.

---

## Overview

This project monitors an induction motor's electrical parameters in real time using an ESP32 microcontroller connected to voltage and current sensors. Live data is transmitted over Wi-Fi to a cloud dashboard (Blynk/ThingSpeak), where voltage, current, power, energy consumption, and efficiency can be viewed from any device.

The project has two components:

1. **IoT System** — Sensor data acquisition, ESP32 firmware, Wi-Fi transmission, cloud dashboard
2. **Efficiency Analysis** — Comparison of input electrical power vs. output mechanical power under varying load conditions

---

## System Architecture

```
  3-Phase Induction Motor
          |
  +-------+--------+
  |                |
ZMPT101B        ACS712
Voltage Sensor  Current Sensor
  |                |
  +-------+--------+
          |
        ESP32
    (Wi-Fi + ADC)
          |
        Wi-Fi
          |
   Blynk / ThingSpeak
          |
   Mobile / Laptop Dashboard
```

---

## Monitored Parameters

| Parameter | Sensor | Unit |
|-----------|--------|------|
| Voltage | ZMPT101B | V (RMS) |
| Current | ACS712 (20A) | A (RMS) |
| Active Power | Calculated | W |
| Energy Consumption | Integrated | kWh |
| Power Factor | Estimated | — |
| Efficiency | Calculated | % |

### Efficiency Calculation

```
Input Power  = Vrms x Irms x Power Factor       [W]
Output Power = (2 x pi x RPM x Torque) / 60     [W]

Efficiency = (Output Power / Input Power) x 100  [%]
```

---

## Hardware Components

| Component | Model | Purpose |
|-----------|-------|---------|
| Microcontroller | ESP32 (30-pin) | Data acquisition and Wi-Fi |
| Voltage Sensor | ZMPT101B | AC voltage measurement |
| Current Sensor | ACS712-20A | AC current measurement |
| Motor | 3-phase Induction Motor | Device under test |
| Power Supply | 5V USB adapter | ESP32 power |
| Breadboard and Jumper wires | — | Connections |

---

## Software Stack

| Tool | Purpose |
|------|---------|
| Arduino IDE | ESP32 firmware development |
| Blynk 2.0 / ThingSpeak | IoT cloud dashboard |
| Microsoft Excel | Efficiency graphs and data analysis |

---

## Firmware Logic

```
Loop every 1 second:
  1. Read raw ADC from ZMPT101B  ->  calculate Vrms
  2. Read raw ADC from ACS712    ->  calculate Irms
  3. Compute Power  = Vrms x Irms x PF
  4. Integrate Energy += Power x dt / 3600000
  5. Send {V, I, P, E, efficiency} to Blynk/ThingSpeak
  6. Print to Serial Monitor
```

---

## Repo Structure

```
iot-energy-monitoring/
|-- src/
|   |-- energy_monitor.ino        # Main ESP32 sketch
|   |-- sensor_calibration.ino    # Sensor offset calibration
|   `-- blynk_config.h            # Blynk auth token and Wi-Fi credentials
|-- docs/
|   |-- block_diagram.png
|   |-- circuit_diagram.png
|   `-- efficiency_graphs.xlsx
|-- report/
|   `-- IoT_Energy_Monitoring_Report.pdf
`-- README.md
```

---

## Testing Plan

- [ ] Sensor calibration (no-load readings)
- [ ] No-load test — measure current and voltage with motor running free
- [ ] Full-load test — apply rated load, record all parameters
- [ ] Overload test — exceed rated load, observe parameter changes
- [ ] Efficiency curve — plot efficiency vs. load (25%, 50%, 75%, 100%)

---

## Expected Results

| Load | Voltage (V) | Current (A) | Power (W) | Efficiency (%) |
|------|-------------|-------------|-----------|----------------|
| No load | 230 | 0.8 | 180 | 55 |
| 50% load | 229 | 1.8 | 380 | 78 |
| Full load | 228 | 2.4 | 490 | 87 |

---

## Applications

- Industrial motor health monitoring
- Factory energy auditing
- Predictive maintenance systems
- Smart grid integration

---

## Course

BEEE — AC Machines  
B.Tech Electrical and Electronics Engineering  
VIT Chennai | 2026–27

**Team:** Sanjay Kumar G (24BEE1256)

---

## Status

In progress — hardware connections and firmware development ongoing.
