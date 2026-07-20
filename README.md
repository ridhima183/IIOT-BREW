<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:722F37,50:A9746E,100:D8A48F&height=220&section=header&text=IIoT%20Lab%20Notebook&fontSize=42&fontColor=FFF5E9&animation=fadeIn&fontAlignY=35&desc=15%20Industrial%20IoT%20Problem%20Statements%20%E2%80%A2%20Arduino%20%2B%20Tinkercad&descAlignY=55&descSize=16" width="100%"/>

<img src="https://img.shields.io/badge/Board-Arduino%20Uno-A9746E?style=for-the-badge&logo=arduino&logoColor=FFF5E9" />
<img src="https://img.shields.io/badge/Simulated%20on-Tinkercad-722F37?style=for-the-badge&logo=autodesk&logoColor=FFF5E9" />
<img src="https://img.shields.io/badge/Domain-Industrial%20IoT-D8A48F?style=for-the-badge&logo=internetarchive&logoColor=3B2A2A" />
<img src="https://img.shields.io/badge/Status-Actively%20Brewing-EADBC8?style=for-the-badge&logo=coffeescript&logoColor=3B2A2A" />

<br/>

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=18&duration=2800&pause=900&color=A9746E&center=true&vCenter=true&width=560&lines=15+sensors.+15+circuits.+1+cozy+repo.;Where+factories+meet+breadboards.;Sensing+%E2%80%A2+Alerting+%E2%80%A2+Automating.;Built+with+chai+and+curiosity+%E2%98%95" alt="Typing SVG" />

</div>

<br/>

## 🧵 What's in the basket?

A comfy little collection of **15 Industrial IoT (IIoT) problem statements**, each wrapped up with a real-world **problem**, a plain-English **description**, a plug-and-play **Tinkercad circuit walkthrough**, and clean, ready-to-flash **Arduino (C++) code**.

Think of it as a lab notebook — the kind with dog-eared corners and coffee rings — for anyone learning how sensors, actuators, and a humble Arduino Uno can quietly keep a factory floor safe, efficient, and self-aware.

<div align="center">
<img src="https://img.shields.io/badge/🌡️-Temperature-D8A48F?style=flat-square" />
<img src="https://img.shields.io/badge/💨-Gas%20%26%20Smoke-A9746E?style=flat-square" />
<img src="https://img.shields.io/badge/📳-Vibration-722F37?style=flat-square" />
<img src="https://img.shields.io/badge/⚡-Energy-D8A48F?style=flat-square" />
<img src="https://img.shields.io/badge/💧-Liquid%20Level-A9746E?style=flat-square" />
<img src="https://img.shields.io/badge/🏭-Automation-722F37?style=flat-square" />
</div>

---

## 📖 Table of Contents

| # | Problem Statement | Core Sensor(s) | Focus Area |
|---|---|---|---|
| 01 | [Temperature & Humidity Monitoring](./01_temp_humidity_monitoring.md) | DHT11 | Environmental Safety |
| 02 | [Gas Leak Detection](./02_gas_leak_detection.md) | MQ-2 | Hazard Prevention |
| 03 | [Machine Vibration Monitoring](./03_machine_vibration_monitoring.md) | SW-420 | Predictive Maintenance |
| 04 | [Smart Energy Meter](./04_smart_energy_meter.md) | ACS712 | Power Management |
| 05 | [Water/Chemical Tank Level Monitoring](./05_water_tank_level_monitoring.md) | HC-SR04 | Process Automation |
| 06 | [Conveyor Belt Counting & Jam Detection](./06_conveyor_belt_object_counting.md) | IR Sensor | Production Line |
| 07 | [Fire & Smoke Detection](./07_fire_smoke_detection.md) | Flame + MQ-2 | Emergency Response |
| 08 | [Air Quality Monitoring](./08_air_quality_monitoring.md) | MQ-135 | Worker Safety |
| 09 | [RFID Asset Tracking & Access Control](./09_rfid_asset_tracking.md) | MFRC522 | Security & Logistics |
| 10 | [Motor Speed (RPM) Monitoring](./10_motor_speed_monitoring.md) | IR Slotted Sensor | Machine Health |
| 11 | [Smart Warehouse Lighting](./11_smart_warehouse_lighting.md) | LDR + PIR | Energy Efficiency |
| 12 | [Warehouse Humidity Control](./12_warehouse_humidity_control.md) | DHT11 | Storage Quality |
| 13 | [Load Cell Weight Monitoring](./13_load_weight_monitoring.md) | HX711 (simulated) | Quality Control |
| 14 | [Restricted Zone Intrusion Detection](./14_restricted_zone_security.md) | PIR + Button | Industrial Security |
| 15 | [Combined Predictive Maintenance](./15_predictive_maintenance_combined.md) | DHT11 + SW-420 | Multi-Sensor Fusion |

---

## 🧶 Each file unravels into

```
📄 problem_statement.md
├── 🎯 Problem Statement     → the real-world pain point
├── 📝 Description           → how the solution works, in plain words
├── 🔩 Components Required   → the shopping list
├── 🧪 Tinkercad Circuit     → step-by-step wiring + how to simulate it
└── 💻 Arduino Code          → copy, paste, upload, done
```

---

## 🛠️ Tech Stack & Tools

<div align="center">

<img src="https://img.shields.io/badge/C++-722F37?style=for-the-badge&logo=cplusplus&logoColor=FFF5E9" />
<img src="https://img.shields.io/badge/Arduino%20IDE-A9746E?style=for-the-badge&logo=arduino&logoColor=FFF5E9" />
<img src="https://img.shields.io/badge/Tinkercad%20Circuits-D8A48F?style=for-the-badge&logo=autodesk&logoColor=3B2A2A" />
<img src="https://img.shields.io/badge/Markdown-EADBC8?style=for-the-badge&logo=markdown&logoColor=3B2A2A" />

</div>

**Common hardware across the set:** Arduino Uno · DHT11 · MQ-2 / MQ-135 · HC-SR04 · SW-420 · ACS712 · IR sensors · MFRC522 RFID · PIR · LDR · relay modules · buzzers · LEDs

---

## 🚀 How to use this repo

1. **Pick a problem statement** from the table above — open its `.md` file.
2. **Build the circuit** in [Tinkercad Circuits](https://www.tinkercad.com/circuits) following the wiring notes (each file tells you exactly which pin goes where).
3. **Paste the code** into Tinkercad's Text-mode code editor (or your Arduino IDE if you're working with real hardware).
4. **Hit Start Simulation ▶️**, nudge the sensor sliders/toggles, and watch the Serial Monitor light up with live readings and alerts.
5. Tweak thresholds, swap LEDs for relays, or chain a few problem statements together for a mini smart-factory demo. 🏭✨

> 💡 **Tip:** Sensors like the DHT11, gas sensors, and vibration modules aren't physically shakeable in Tinkercad — use their built-in sliders/click-triggers during simulation to mimic real-world changes.

---

## 🎨 Palette Notes

This README (and its siblings) lean into a warm, vintage **burgundy × terracotta** palette — because even circuit diagrams deserve good aesthetics.

<div align="center">

`#722F37` ██ &nbsp; `#A9746E` ██ &nbsp; `#D8A48F` ██ &nbsp; `#EADBC8` ██ &nbsp; `#FFF5E9` ██ &nbsp; `#3B2A2A` ██

</div>

---

<div align="center">

### 🧡 Made with breadboards, chai, and a soft spot for well-labeled wires.

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:D8A48F,50:A9746E,100:722F37&height=120&section=footer" width="100%"/>

</div>
