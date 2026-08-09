# 🛡️ ESP32 Gas & Proximity Safety Alarm System

A simple environmental safety monitoring project built using an **ESP32-DEVKITC-32E** microcontroller. The system continuously measures gas concentration and obstacle distance, triggering a relay switch, buzzer alarm, and status LED whenever a hazard threshold is crossed.

---

## 📌 Project Overview
This project combines gas leakage detection, ultrasonic distance sensing, and high-power load control into a single PCB. It is powered directly from a 100V–240V AC wall supply using a compact HLK-10M05 AC-to-DC converter module with built-in fuse and surge protection.

---

## 🚀 Key Features
* ⛽ **Gas Detection**: Reads combustible gas and smoke levels using an MQ-04 gas sensor module (Analog `A0` on `IO34` & Digital `D0` on `IO35`).
* 📏 **Distance Sensing**: Measures object proximity using an HC-SR04 ultrasonic distance sensor (`TRIG` & `ECHO` on `IO18`).
* ⚡ **Relay Control**: Switches a 5V SPDT relay (`RELAY` on `IO23`) via a transistor driver to control external loads.
* 🔔 **Audible & Visual Alarms**: Activates a 5V piezo buzzer and status LED simultaneously (`INP` on `IO21`).
* 🔌 **Direct AC Mains Powered**: Powered by an HLK-10M05 AC-DC module with a 1A fuse and varistor protection.

---

## 🧩 Hardware Components Used
* **Microcontroller**: ESP32-DEVKITC-32E
* **Power Supply**: HLK-10M05 (AC 230V to DC 5V 2A)
* **Protection**: 1A 250V Fuse & B72220S0301K101 Varistor (MOV)
* **Sensors**: MQ-04 Gas Sensor Module & HC-SR04 Ultrasonic Distance Sensor
* **Actuators & Indicators**: SRD-05VDC-SL-C 5V Relay, KSSG1201-16 Piezo Buzzer, 15400594F3590 LED
* **Drivers & Logic**: BD139G NPN Transistors (Q1, Q2), 1N4007 Diodes (D1, D2), Resistors (10kΩ, 15kΩ, 1kΩ, 330Ω)

---

## ⚙️ How It Works
1. **Power**: 230V AC mains is converted to 5V DC by the HLK-10M05 module (`EXT_5V`) to power the ESP32 and sensors.
2. **Sensing**: 
   - The **MQ-04 Gas Sensor** sends gas levels to ESP32 Pin `J2 5 (IO34)` via net `A0` and `J2 6 (IO35)` via net `D0`.
   - The **Ultrasonic Sensor** measures distance via `TRIG` and `ECHO` connected to ESP32 Pin `J3 9 (IO18)`.
3. **Alert Action**:
   - If gas is detected OR an object is too close:
     - The **Buzzer & Status LED** (`INP` on `J3 6 / IO21`) activate together to sound an alarm and blink the LED.
     - The **Relay** (`RELAY` on `J3 2 / IO23`) switches ON to power external safety devices.

---

## 🔌 Exact Circuit Schematic Pin Connections

| ESP32 Symbol Pin (U1) | Pin Label on U1 | Connected Net Label | Target Module / Component Pin | Circuit Function |
| :--- | :--- | :--- | :--- | :--- |
| **`J2 5`** | `IO34` | **`A0`** | MQ-04 Pin 4 (`A0`) via `R2 (15k)` / `R6 (15k)` divider | Gas Sensor Analog Input |
| **`J2 6`** | `IO35` | **`D0`** | MQ-04 Pin 3 (`D0`) via `R3 (10k)` / `R5 (15k)` divider | Gas Sensor Digital Input |
| **`J2 14`** | `GND_J2_14` | **`GD`** | MQ-04 Pin 2 (`GND`) | Gas Sensor Ground |
| **`J2 19`** | `EXT_5V` | **`EXT_5V`** | HLK-10M05 Output (`+VO`) / `+5` Power Rail | Main 5V DC Power Input |
| **`J3 2`** | `IO23` | **`RELAY`** | Relay Driver (`R7 1k` $\rightarrow$ `Q1 BD139G Base`) | Relay Control Output |
| **`J3 6`** | `IO21` | **`INP`** | Buzzer (`R8 1k` $\rightarrow$ `Q2`) & LED (`R9 330` $\rightarrow$ `D3`) | Shared Buzzer & LED Alarm Output |
| **`J3 7`** | `GND_J3_7` | **`GND`** | Ultrasonic `GND`, Transistors Emitters, LED Cathode | Main System Ground Rail |
| **`J3 9`** | `IO18` | **`TRIG`** & **`ECHO`** | Ultrasonic `TRIG` (Pin 2) & `ECHO` (Pin 3 via `R1 10k`/`R4 15k`) | Ultrasonic Distance Measurement |

---
markdown


## 🚧 Challenges Faced & Solutions
### 1. Custom Integrated Library Creation (`.IntLib` / `.LibPkg`)
* **Challenge**: There were no pre-existing integrated libraries available for several core components, including the **MQ-04 Gas Sensor Module**, **HC-SR04 Ultrasonic Sensor Module**, and **Glass Tube Fuseholder**.
* **Solution**: Built custom **Schematic Libraries (`.SchLib`)** from scratch for each symbol, created exact **PCB Footprint Libraries (`.PcbLib`)** following IPC pad/pitch specifications, sourced 3D STEP CAD models online, aligned the 3D bodies to the pads, and compiled a custom **Integrated Library Package (`.LibPkg`)**.
### 2. Single-Layer PCB Trace Routing Constraints
* **Challenge**: Designing and routing the entire PCB under strict **Single-Layer (Single-Sided PCB)** manufacturing constraints without signal line crossovers.
* **Solution**: Strategic component placement was planned to ensure short net paths, routing 5V power and ground buses along outer boundaries, and optimizing trace widths to handle high-voltage 230V AC lines, high-current relay coils, and sensitive 3.3V logic signals on a single copper layer.
 
---
