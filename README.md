# fakeTec_pcb - 5.8GHz FSK Edition

Experimental 5.8GHz FSK nRF52840 board in the Heltec V3 form factor. Designed with [Reticulum](https://reticulum.network/) and [Meshtastic®](https://meshtastic.org/) in mind.

> **Project status: HARDWARE ONLY. There is no firmware yet.**
> No software exists for this radio in Meshtastic or Reticulum. This repository provides a routed, ready-to-solder PCB as a proof of concept, and an open invitation to write the drivers for the AMICCOM A5133 chip. See [Firmware & Development](#firmware--development).

> **Legal disclaimer:** The end user is solely responsible for ensuring that the assembly, configuration and operation of these PCBs comply with the telecommunications laws and regulations of their country or region. See [Regulatory notes](#regulatory-notes). No warranty is provided; you build and operate this hardware at your own risk.

*Meshtastic® is a registered trademark of Meshtastic LLC. This project is not affiliated with or endorsed by Meshtastic LLC.*

---

## Table of contents

- [Project status](#project-status)
- [What is this?](#what-is-this)
- [Features](#features)
- [Hardware overview](#hardware-overview)
- [Bill of materials](#bill-of-materials)
- [Assembly guide](#assembly-guide)
- [Firmware & Development](#firmware--development)
- [Regulatory notes](#regulatory-notes)
- [Changelog](#changelog)
- [License](#license)
- [Credits](#credits)

---

## Project status

| Item | Status | Notes |
| :--- | :---: | :--- |
| PCB design (schematic + layout) | ✅ Done | Fully routed. |
| PCB fabricated | ❌ Not tested| |
| Assembly tested | ❌ Not tested | |
| SPI communication with A5133 | ❌ Not tested | Reading the chip ID/register is the first milestone. |
| TX / RX between two nodes | ❌ Not tested | |
| Firmware (Reticulum / Meshtastic) | ❌ Not started | Help wanted. |

Legend: ✅ done · 🚧 in progress · ❌ not done

---

## What is this?

A hardware fork of the [fakeTec_pcb](https://github.com/gargomoma/fakeTec_pcb) family, derived from [lupusworax's variant](https://github.com/gargomoma/fakeTec_pcb/issues/16), which is in turn based on the original design by [gargomoma](https://github.com/gargomoma).

Where the original boards use sub-GHz LoRa or 2.4GHz radios, this version replaces the RF section with a **Vollgo VG6244S580X0M1-IPEX** module built around the **AMICCOM A5133** transceiver. It drops LoRa modulation in favor of **5.8GHz FSK**, aiming at high-throughput local links instead of long range.

**Expected characteristics (nominal, from chip documentation, not measured on this board):**

- Data rates from 500 kbps up to 4 Mbps.
- Short-to-medium range. 5.8GHz needs near line of sight and is much more affected by walls and vegetation than sub-GHz LoRa.

---

## Features

- **5.8GHz FSK transceiver:** Vollgo VG6244S580X0M1-IPEX (AMICCOM A5133).
- **nRF52840 MCU:** ProMicro / SuperMini nRF52840 board. See the compatibility note below.
- **Heltec V3 form factor:** fits existing community cases for the original fakeTec / Heltec V3. Minor modifications may be needed because of the module footprint.
- **I2C expansion:** dedicated side pads for a standard SSD1306 OLED.
- **Battery monitoring:** voltage divider with 1206 SMD resistors.
- **Boost converter footprint (HW-085):** supplies the voltage required by the RF module.
- **Buttons:** User and Reset.

---

## Hardware overview

### Specifications

| Parameter | Value |
| :--- | :--- |
| MCU | nRF52840 (ProMicro / SuperMini). |
| RF module | Vollgo VG6244S580X0M1-IPEX (AMICCOM A5133) |
| RF frequency | 5.8GHz ISM band. TODO: exact tuning range. |
| Modulation | FSK |
| Max TX power | 25 dBm |
| RF module supply voltage | 5 |
| Battery | 1S Litium |
| Antenna connector | IPEX / u.FL |
| Board size | Same as original faketec |

### Documentation

- Schematic: [Link](https://github.com/sqtactical/FakeTec_5.8G/blob/main/Faketec_5.8G_schematic.pdf)
- Gerbers: [Link](https://github.com/sqtactical/FakeTec_5.8G/blob/main/Faketec_5.8G_v1.0.zip)
- Vollgo VG6244S580X0M1 datasheet: [LCSC](https://www.lcsc.com/datasheet/C7465093.pdf)
- AMICCOM A5133 datasheet: [JLCPCB](https://jlcpcb.com/partdetail/AMICCOMElec-A5133/C3035807)


---

## Bill of materials

| Part | Qty | Spec / source example | Approx. cost | Notes |
| :--- | :---: | :--- | :--- | :--- |
| ProMicro / SuperMini nRF52840 | 1 | [AliExpress example](https://es.aliexpress.com/item/1005009116054547.html) | ~€5.00 | Main MCU. Pinouts vary between clones: TODO list tested boards. |
| Vollgo VG6244S580X0M1-IPEX | 1 | [LCSC](https://www.lcsc.com/product-detail/C7465093.html) | 5.2€ | 5.8GHz FSK module (A5133). |
| Resistor 680kΩ, 1206 | 1 | [AliExpress example](https://es.aliexpress.com/item/1005002991902748.html) | <€0.50 | Battery divider. TODO: confirm which position (R_top / R_bottom). |
| Resistor 1MΩ, 1206 | 1 | same as above | | Battery divider. |
| HW-085 boost converter | 1 | [AliExpress example](https://es.aliexpress.com/item/1005007013856492.html) | <€1 | Boost for the RF module. TODO: output voltage and placement. |
| SSD1306 OLED, I2C (optional) | 1 | [AliExpress example](https://es.aliexpress.com/item/1005005970901119.html) | ~€1.50 | Insulate underneath (see assembly). |
| Push buttons 3x4x2.0, 2-pin | 2 | [AliExpress example](https://es.aliexpress.com/item/4001125532910.html) | <€0.50 | User and Reset. |
| 5.8GHz antenna + IPEX pigtail | 1 | AliExpress / supplier | ~€5.00 | It must be a **5.8GHz** antenna. Never use 2.4GHz or sub-GHz antennas. |
| PCB | 1 | Gerbers | Variable | JLCPCB, PCBWay, etc. |

*Links are non-affiliated examples and may expire. Use the specs in the table to find equivalent parts.*

---

## Assembly guide

> **CRITICAL: never power on or transmit without a 5.8GHz antenna connected to the module.** Operating without an antenna can permanently damage the RF power amplifier.

Assemble in order of increasing height, lowest-profile parts first.

### Step 1: SMD resistors and buttons

- Solder the two 1206 resistors of the battery divider in their positions.
- Solder the two tactile buttons (User and Reset).

### Step 2: Vollgo 5.8GHz module

Check orientation and alignment before applying heat.

- **Hot plate (recommended):** apply solder paste, align the module and reflow.
- **Pin by pin:** tack two opposite corner pads, apply generous flux and solder the rest one by one. Preheating the board to 80-90ºC helps. Keep solder wick at hand for bridges.

### Step 3: HW-085 boost converter

Solder it on the same face as the 1206 resistors, carefully ensuring the solder touches the pads

### Step 4: Microcontroller

Solder the ProMicro directly to the PCB, or use low-profile female headers if you want it removable.

### Step 5: OLED (optional)

- Place a layer of **Kapton or electrical tape** under the screen to prevent its pins from shorting against the MCU board.
- The VCC/GND pin order differs between SSD1306 module manufacturers. **Check yours against the pads before soldering.**

### Step 6: Pre-power checks

Do these before connecting a battery or USB:

1. Check continuity between the supply rail and GND. There must be no short.
2. Check battery polarity against the pad markings.
3. Connect the antenna.

### Step 7: Verification checklist

- [ ] The MCU boots (serial output or bootloader detected).
- [ ] The booster output voltage is as expected.
- [ ] The battery reading is correct (compare with a multimeter).
- [ ] The OLED works (if fitted).
- [ ] The A5133 responds over SPI (register or ID read). Requires test code.

<img width="842" height="577" alt="Assembled fakeTec 5.8GHz board" src="https://github.com/user-attachments/assets/d42ff00a-c1bd-45c6-a2a5-ae071c2a1491" />


---

## Firmware & Development

There is no ready-to-use firmware for this hardware. The A5133 is an FSK transceiver and is not compatible with the Semtech radios (SX126x, SX128x) that standard Meshtastic and Reticulum builds support, so those builds **will not work** on this board.

What this means for each project:

- **Reticulum:** more flexible regarding network interfaces. Probably the more realistic first target.
- **Meshtastic:** its radio layer is built around LoRa. Supporting an FSK radio likely means a new radio interface and a review of timing and protocol assumptions, not only a new driver.

### Suggested milestones

1. SPI bring-up: read the A5133 ID/registers from the nRF52840.
2. Basic TX/RX (ping-pong) between two boards.
3. Link characterization (throughput, packet loss, range).
4. Reticulum interface.
5. Meshtastic integration (feasibility to be assessed).

### How to help

- Look for issues labeled `help wanted`.
- When you get any milestone working, open a PR or issue with the results and the code.

---

## Regulatory notes

- The 5.8GHz band is shared with Wi-Fi 5GHz, FPV video links and amateur radio, among others.
- Maximum power, allowed channels and duty cycle depend on your country. In the EU, check the applicable regulation for your country before transmitting. TODO: add references.
- The maximum TX power of the module is 25 dBm. Limit the output power in firmware to the legal limit once firmware exists.
- Using this board with an antenna other than a 5.8GHz one may damage the module.

---

## Changelog

### 5G8-v1.0: initial 5.8GHz release (hardware only)

- Replaced the LoRa footprints with the Vollgo VG6244S580X0M1-IPEX (5.8GHz FSK).
- Re-routed SPI/RF traces and pads for the Vollgo module and the A5133 pinout.
- Status: see [Project status](#project-status).

---

## Credits

- **[gargomoma](https://github.com/gargomoma):** original open-source `fakeTec_pcb` layout and concept.
- **[lupusworax](https://github.com/gargomoma/fakeTec_pcb/issues/16):** intermediate design improvements.
- **[EmilioAl](https://github.com/EmilioAL-Git)** and contributors: adaptation to 5.8GHz FSK (Vollgo VG6244).
