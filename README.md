# LoRa Solar Water Heater Monitor

An off-grid, solar-powered IoT solution to monitor the temperature of a rooftop solar water heater and transmit the data down to an apartment using LoRa technology. Integrates seamlessly with **Home Assistant** via **ESPHome**.


## Overview
This project solves the problem of having no Wi-Fi or mains power on a rooftop. 
* **The Roof Node** is completely off-grid. It wakes up every 5 minutes, reads the water temperature and battery voltage, broadcasts it via LoRa, and goes back to deep sleep. It is powered by an 18650 battery and kept charged by a small solar panel.
* **The Apartment Node** sits indoors powered by USB. It listens for the LoRa packets, displays the live data on its built-in OLED screen, and forwards the data to Home Assistant over Wi-Fi.

## Hardware Requirements
* 2x **Heltec WiFi LoRa 32 V3** (ESP32-S3)
* 1x **DS18B20** Waterproof Temperature Sensor
* 1x **4.7kΩ Resistor** (for the DS18B20 data line)
* 1x **HW-736** (or similar) Solar Battery Charging/BMS Board
* 1x **18650 Lithium-Ion Battery** (3.7V)
* 1x **5V Solar Panel**
* 1x Waterproof Enclosure (IP65+)

---

## Wiring Pinouts



### 1. Roof Node Pin Table
| Component | Pin / Pad | Connects To | Notes |
| :--- | :--- | :--- | :--- |
| **Solar Panel** | USB | HW-736 micro usb port| |
| **HW-736** | `SYS+` | Heltec `5V` | Use the SH1.25 battery connector |
| **HW-736** | `SYS-` | Heltec `GND` | Use the SH1.25 battery connector |
| **HW-736** | `BAT+` | 18650 Battery + | *Watch polarity carefully!* |
| **HW-736** | `BAT-` | 18650 Battery - | *Watch polarity carefully!* |
| **DS18B20** | `VCC` (Red) | Heltec `3.3V` | |
| **DS18B20** | `VCC` (Red) | 4.7kΩ Resistor | use pull up resistor |
| **DS18B20** | `DATA` (Yellow) | Heltec `GPIO7` | Sensor data line. |
| **DS18B20** | `DATA` (Yellow) | 4.7kΩ Resistor  | The other end of the pull up resistor connected to VCC |
| **DS18B20** | `GND` (Black) | Heltec `GND` | Ground for sensor. |

*(Note: The LoRa antenna must be securely attached before powering on to prevent radio damage).*

---

### 2. Apartment Node (Receiver / Wi-Fi Gateway)

The Apartment node requires no external wiring other than a standard USB-C cable for power. It utilizes the Heltec V3's internal hardware.

--- 


## Installation & Flashing
1. Add the `roof-node.yaml` and `apartment-node.yaml` files to your **Home Assistant ESPHome Add-on**.
2. Flash the **Apartment Node** first. 
3. Flash the **Roof Node** via USB (Ensure you remove the deep sleep block temporarily if you want to read USB logs).
4. Once both are flashed, press the `RST` button on the Roof Node to force a packet transmission and verify the OLED screen on the Apartment Node updates.
5. In Home Assistant, navigate to **Settings > Devices & Services** and configure the newly discovered `apartment-node`.
