# fakeTec_pcb - 5.8GHz FSK Edition

An experimental, high-speed 5.8GHz nRF52 device with the form factor of the Heltec v2 & v3 devices. The hardware is designed with [Reticulum](https://reticulum.network/) and [Meshtastic®](https://meshtastic.org/) in mind.

This repository is a hardware fork derived from **[lupusworax's variant](https://github.com/gargomoma/fakeTec_pcb/issues/16)**, which was originally based on the excellent base design by **[gargomoma](https://github.com/gargomoma)**.

> 🚨 **CRITICAL NOTICE: HARDWARE ONLY - NO FIRMWARE YET!**  
> At this moment, there is **ZERO software developed** for this exact radio configuration in either Meshtastic or Reticulum. This repository provides a fully routed, ready-to-solder PCB proof-of-concept. It is an open invitation for developers to write the necessary drivers for the AMICCOM A5133 chip.

> ⚠️ **Legal Disclaimer:**  
> **🇬🇧:** It is the sole responsibility of the end user to ensure that the assembly, configuration, and operation of these PCBs comply with applicable local telecommunications laws and regulations regarding the 5.8GHz ISM band.

## 🚀 The 5.8GHz FSK Difference
While the original versions focus on sub-GHz LoRa modules or 2.4GHz variants, this experimental version swaps the RF section out for a **Vollgo VG6244S580X0M1-IPEX** module. 

Powered by the **AMICCOM A5133** transceiver, this board drops LoRa modulation in favor of **5.8GHz FSK**, enabling blazing-fast local mesh data rates ranging from **500 kbps up to 4 MB/s**. 

## ✨ Features
- **5.8GHz FSK Transceiver:** Built around the Vollgo VG6244S580X0M1-IPEX (AMICCOM A5133) module for high-speed, short-to-medium range data transfers.
- **Reticulum & Meshtastic Ready (Hardware):** Pinout and layout optimized for future mesh network firmware implementations.
- **Heltec v3 Form Factor:** Fits into existing community 3D-printed cases meant for the original FakeTec or Heltec v3 (minor case modifications may be needed depending on the module footprint).
- **nRF52840 MCU:** Powered by the affordable and highly capable ProMicro / SuperMini nRF52840 board.
- **I2C Expansion:** Dedicated side ports ready to accept a standard SSD1306 OLED screen.
- **Battery Monitoring:** Integrated battery voltage sensing via 1206 SMD resistors.

## 📜 Changelog

### [v1.0] - *Initial 5.8GHz Release (Hardware only)*
- **RF Module Migration:** Replaced previous LoRa footprints with the **Vollgo VG6244S580X0M1-IPEX** (5.8GHz FSK).
- **PCB Layout Updates:** Re-routed SPI/RF traces and pad layouts to accommodate the Vollgo module footprint and the AMICCOM A5133 pinout.
- **Software Status:** Hardware validated, pending community firmware development.

## 🛒 Bill of Materials (BOM)

| Part | Source / Example | Approx. Cost | Note |
| :--- | :--- | :--- | :--- |
| **ProMicro nRF52840** (SuperMini) | [AliExpress](https://es.aliexpress.com/item/1005009116054547.html) | ~€5.00 | Main microcontroller. |
| **Vollgo VG6244S580X0M1-IPEX** | AliExpress / Supplier | Variable | 5.8GHz FSK module (AMICCOM A5133). |
| **1x 680K and 1x 1MΩ Resistors** | [AliExpress](https://es.aliexpress.com/item/1005002991902748.html) | <€0.50 | Battery voltage divider. SMD 1206 size. |
| **HW-085 boost converter** | [AliExpress](https://es.aliexpress.com/item/1005007013856492.html) | <€1 | Boost to allow the operation of the FSK module. |
| **OLED SSD1306 I2C** *(Optional)* | [AliExpress](https://es.aliexpress.com/item/1005005970901119.html) | ~€1.50 | Add insulating tape beneath to prevent MCU shorts. |
| **2x Push Buttons** | [AliExpress](https://es.aliexpress.com/item/4001125532910.html) | <€0.50 | Search for "3x4x2.0 2 Pin Buttons" (User & Reset). |
| **5.8GHz Antenna & Pigtail** | AliExpress / Supplier | ~€5.00 | Ensure you use a dedicated **5.8GHz** antenna (Do NOT use 2.4GHz or Sub-GHz antennas!). |
| **Custom PCB** | Custom Gerber | Variable | Order via your favorite fabrication house (JLCPCB, PCBWay, etc.). |

*(Note: Links are non-affiliated and provided only as examples)*

## 🛠️ Mounting & Assembly Instructions

> ⚠️ **CRITICAL WARNING:** NEVER power on or transmit with the board without a 5.8GHz antenna connected to the Vollgo module! Operating without an antenna will permanently damage the RF power amplifier.

To ensure a clean and hassle-free build, assemble the board in order of component height (lowest profile components first):

### 1️⃣ Step 1: Vollgo 5.8GHz Module Soldering
Carefully align the **VG6244S580X0M1-IPEX** module over its footprint.
- **Method A (Hot Plate - Recommended):** Apply solder paste to the pads, align the module carefully, and reflow on a hot plate.
- **Method B (Pin-by-Pin):** Tack down two opposite corner pads to anchor the module. Apply generous flux and carefully solder each pad one by one. Preheating the board (80-90ºC) is highly recommended. Have solder wick ready to clear any bridges.

### 2️⃣ Step 2: Microcontroller (MCU)
- **ProMicro / SuperMini nRF52840:** Solder the MCU board directly to the PCB, or use low-profile female pin headers if you want it to be removable.

### 3️⃣ Step 3: SMD Components & Small Hardware
- **Battery Divider Resistors:** Solder the two SMD 1206 resistors (1MΩ / 1MΩ, or 680KΩ / 1MΩ) in their designated locations.
- **Push Buttons:** Solder the two SMD tactile buttons (User & Reset).

### 4️⃣ Step 4: Screen & Finishing Touches
- **SSD1306 OLED Display (Optional):** If mounting a display directly above the MCU, **you must place a layer of insulating Kapton or electrical tape** underneath the screen module to prevent its pins from shorting against the nRF52 board.
- **Antenna Connection:** Snap on your IPEX pigtail and secure your **5.8GHz antenna** before plugging in USB or battery power.

## 💻 Firmware & Software Development (Call for Devs!)

**As mentioned above, there is currently no ready-to-use firmware for this specific hardware combo.** 

Because this board uses a 5.8GHz AMICCOM A5133 FSK radio instead of the traditional Semtech SX1262 (LoRa) or SX1280 (2.4GHz), standard Meshtastic or Reticulum builds **will not work** out of the box. 

If you are a developer looking to expand the Reticulum or Meshtastic ecosystems into the high-bandwidth 5.8GHz spectrum, this hardware is ready for you. 

> 👨‍💻 **Pinout Reference:** When writing your drivers, ensure your pin mapping accurately reflects the SPI, BUSY/IRQ, DIO, and RESET lines between the ProMicro and the Vollgo module footprint! Schematics are readily available

## 🤝 Credits & Thanks
- **[gargomoma](https://github.com/gargomoma)** for creating the original open-source `fakeTec_pcb` layout and concept.
- **[lupusworax](https://github.com/gargomoma/fakeTec_pcb/issues/16)** for the intermediate design improvements.
- Designed and modified for 5.8GHz FSK (Vollgo VG6244) by **[EmilioAl](https://github.com/EmilioAL-Git)** & contributors.

---
**Disclaimer:** *No warranty is provided. You build and operate this hardware entirely at your own risk. Always ensure compliance with local radio frequency regulations regarding 5.8GHz transmission power limits and duty cycles.*
