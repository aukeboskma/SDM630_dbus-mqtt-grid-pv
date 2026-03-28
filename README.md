# SDM630 ESPHome MQTT Bridge for Victron and Home Assistant

![ESPHome](https://img.shields.io/badge/ESPHome-Compatible-blue)
![MQTT](https://img.shields.io/badge/MQTT-Ready-green)
![Victron](https://img.shields.io/badge/Victron-Compatible-orange)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

A lightweight ESPHome project that reads an **Eastron SDM630 energy meter** via **RS485 / Modbus** and publishes Victron-compatible **MQTT JSON**.

Designed to work directly with:

- `venus-os_dbus-mqtt-pv`
- `venus-os_dbus-mqtt-grid`

but it also works perfectly as a **generic MQTT energy meter for Home Assistant or other systems**.

---

# Table of Contents

- Overview
- Features
- Example JSON Output
- Hardware
- Wiring Diagram
- MQTT Topics
- Installation
- Diagnostics
- Customization
- License

---

# Overview

This project runs on **ESP8266 or ESP32 using ESPHome** and reads an **SDM630 Modbus meter**.

Instead of publishing dozens of Modbus registers, it only reads the **minimum values required** to build the JSON payload expected by Victron MQTT scripts.

This keeps the Modbus traffic small and allows **fast 1‑second updates**.

The ESP then publishes the data to MQTT as structured JSON.

---

# Features

✔ Fast **1 second polling**  
✔ Minimal Modbus traffic  
✔ Compatible with **Victron dbus-mqtt-pv / dbus-mqtt-grid**  
✔ Works with **Home Assistant MQTT**  
✔ Optional diagnostics logging  
✔ RS485 watchdog monitoring  
✔ Easy switching between **PV and Grid modes**

---

# Example JSON Output

Example output for **PV mode**:

```json
{
  "pv": {
    "power": 5546,
    "current": 23.22,
    "energy_forward": 32926.346,
    "L1": {
      "power": 1843.7,
      "voltage": 237.9,
      "current": 7.75,
      "frequency": 49.94
    },
    "L2": {
      "power": 1853.6,
      "voltage": 239.8,
      "current": 7.73,
      "frequency": 49.94
    },
    "L3": {
      "power": 1841.2,
      "voltage": 238.2,
      "current": 7.73,
      "frequency": 49.94
    }
  }
}
```

---

# Hardware

Tested with:

- ESP8266 NodeMCU
- ESPHome
- TTL to RS485 module
- Eastron SDM630
- MQTT broker
- Victron Venus OS

Works with ESP32 as well.

---

# Wiring Diagram

ESP8266 → RS485 module

```
ESP8266        RS485 Module
---------------------------
GPIO13  -----> DI
GPIO12  <----- RO
GPIO14  -----> DE
GPIO14  -----> RE
3.3V    -----> VCC
GND     -----> GND
```

RS485 module → SDM630

```
RS485 A -----> SDM630 A
RS485 B -----> SDM630 B
```

If communication fails, try swapping **A and B**.

---

# MQTT Topics

Main JSON output:

```
victron/sdm630
```

Diagnostics topic:

```
victron/sdm630/debug
```

---

# Installation

1. Install ESPHome
2. Create a new ESP device
3. Copy the YAML from this repository
4. Adjust:
   - WiFi credentials
   - MQTT settings
   - GPIO pins
   - victron_role
5. Flash the ESP
6. Verify MQTT messages

---

# Diagnostics

A diagnostic switch can be enabled to log:

- WiFi reconnects
- MQTT reconnects
- RS485 communication health
- Meter update watchdog

When disabled, logging stays minimal.

---

# Customization

Key options:

```
victron_role: "pv"
```
or

```
victron_role: "grid"
```

Other configurable settings:

- MQTT topic
- polling speed
- UART pins
- diagnostic logging

---

# Home Assistant Usage

Even though this project was built for Victron, it works great in Home Assistant:

- ESPHome sensors remain visible
- MQTT JSON can be used in automations
- fast Modbus polling
- minimal CPU usage

---

# License

MIT License

Use, modify and share freely.
