# MCU Data Logger PCB

![Tool](https://img.shields.io/badge/Tool-KiCad-314CB0?logo=kicad&logoColor=white)
![Layers](https://img.shields.io/badge/Layers-2-blue)
![MCU](https://img.shields.io/badge/MCU-ATmega328P-informational)
![Board Size](https://img.shields.io/badge/Board-50.8%20×%2030.5%20mm-informational)
![License](https://img.shields.io/badge/License-MIT-green)

A compact, battery-powered data logger PCB built around the **ATmega328P**, designed in KiCad. The board integrates an RTC for timestamping and dual EEPROM chips for storage, with all peripherals communicating over I2C. The design was optimized from an initial 4-layer layout down to 2 layers, cutting manufacturing cost from ~$8 to ~$2 per piece without sacrificing functionality.

---

## Table of Contents

- [Overview](#overview)
- [Schematic](#schematic)
- [PCB Layout](#pcb-layout)
- [3D Render](#3d-render)
- [Design Optimization](#design-optimization)
- [Design Specifications](#design-specifications)
- [Bill of Materials](#bill-of-materials)
- [File Structure](#file-structure)
- [License](#license)

---

## Overview

The board is designed to log data autonomously using an ATmega328P microcontroller. A DS1337S real-time clock provides accurate timestamps via I2C, and two 24LC1025 EEPROM chips (totaling 256 KB of storage) hold the logged data on the same bus. The entire system runs off a battery, with decoupling capacitors stabilizing the supply at the MCU.

External connectivity is handled through a dedicated connectors sheet (sheet 2 of the schematic), which breaks out I2C, UART, GPIO, and ICSP headers — allowing the board to interface with sensors or be reprogrammed in-circuit without any additional hardware.

The key engineering challenge of this project was reducing the board from 4 layers to 2 while keeping all signals properly routed in a compact 50.8 × 30.5 mm footprint.

---

## Schematic

The schematic is split across two sheets. Sheet 1 captures the full circuit — MCU, RTC, EEPROM, power, and passive components. Sheet 2 is an embedded sub-sheet dedicated to the external connectors, keeping the top-level diagram uncluttered.

**Sheet 1 — Main Circuit**
<img width="2281" height="1575" alt="schematic_sheet_1" src="https://github.com/user-attachments/assets/b9b0da14-c5d6-4cfc-a398-2c0577040ce6" />


**Sheet 2 — Connectors**
<img width="1961" height="1351" alt="schematic_sheet_2" src="https://github.com/user-attachments/assets/7b9e1d64-0ba6-420e-ba68-3e36e4b89f37" />


Key connections documented in the schematic:

- **ATmega328P (U4)** — main MCU with a 16 MHz external crystal (Y2) and a 10k reset pull-up (R6)
- **DS1337S RTC (U2)** — I2C real-time clock with a 32.768 kHz crystal (Y1) and 10k I2C pull-ups (R1, R2)
- **24LC1025 EEPROM × 2 (U1, U3)** — dual EEPROM chips on the I2C bus with 4.7k pull-ups (R3, R4)
- **Battery (BT1)** — sole power source for the board
- **Status LED (D1, D2)** — with 330 Ω current-limiting resistors (R5, R7)
- **Connectors (Sheet 2)** — I2C header (J1), Serial UART header (J3), GPIO header (J2, 7 digital pins), ICSP header (J4)



## PCB Layout

All 29 components are placed on the front side of the board. The back copper layer (B.Cu) is used as a ground pour, with signal and power routing handled on the front. Power traces run at 0.35 mm and signal traces at 0.2 mm throughout.

<img width="901" height="570" alt="panel_pcb" src="https://github.com/user-attachments/assets/10ee1a72-01fd-4840-b1f5-6d548557f978" />


Routing highlights:

- **39 through vias** used to transition between layers and connect to the ground pour
- **Ground copper pour** applied to B.Cu, stitched to the GND net
- **Power netclass** (Vcc, GND): 0.35 mm trace width, 0.8 mm via diameter
- **Default netclass** (signals): 0.2 mm trace width, 0.6 mm via diameter, 0.3 mm drill



## 3D Render

<img width="1723" height="902" alt="Project 3 MCU Datalogger" src="https://github.com/user-attachments/assets/adc90d9d-ca17-419f-983c-7b2b1b1580ec" />

<img width="1723" height="902" alt="Project 3 MCU Datalogger_back" src="https://github.com/user-attachments/assets/64c44432-5689-4e70-b1da-a77d7984853a" />


---

## Design Optimization

The most significant design decision in this project was reducing the layer count from 4 to 2.

The initial 4-layer stackup was used to handle signal routing and grounding cleanly, with dedicated inner layers for power and ground planes. While functional, a 4-layer board costs roughly 4× more to manufacture than a 2-layer board at low quantities (~$8 vs ~$2 per piece).

To bring it down to 2 layers, the routing strategy was reworked: signals were rerouted to avoid conflicts without relying on inner layers, the ground plane was moved to B.Cu as a copper pour, and via placement was adjusted to maintain connectivity. The result is a board that meets the same electrical requirements at a fraction of the cost — a meaningful difference at any prototype or small-batch quantity.



## Design Specifications

### Board Stackup

| Layer | Type | Thickness | Material |
|---|---|---|---|
| F.Silkscreen | Top Silk Screen | — | — |
| F.Mask | Top Solder Mask | 0.01 mm | Epsilon R: 3.3 |
| F.Cu | Copper | 0.035 mm | — |
| Dielectric 1 | Core | 1.51 mm | FR4 |
| B.Cu | Copper | 0.035 mm | — |
| B.Mask | Bottom Solder Mask | 0.01 mm | Epsilon R: 3.3 |

**Total board thickness:** 1.6 mm | **Solder mask color:** Green

### Board Dimensions

| Property | Value |
|---|---|
| Width | 50.800 mm |
| Height | 30.480 mm |
| Area | 1,548.4 mm² |

### Routing Rules

| Parameter | Power Net | Default Net |
|---|---|---|
| Clearance | 0.25 mm | 0.20 mm |
| Trace width | 0.35 mm | 0.20 mm |
| Via diameter | 0.80 mm | 0.60 mm |
| Via drill | — | 0.30 mm |

### Board Statistics

| Item | Count |
|---|---|
| Total components | 29 |
| SMD components | 20 |
| THT components | 5 |
| Through vias | 39 |
| SMD pads | 88 |
| Through-hole pads | 25 |
| Mounting holes (NPTH) | 4 |



## Bill of Materials

| Reference | Description | Value | Qty |
|---|---|---|---|
| U4 | Microcontroller, TQFP-32 | ATmega328P-AU | 1 |
| U2 | Real-time clock, SOIC-8 | DS1337S | 1 |
| U1, U3 | EEPROM 1 Mbit, I2C, SOIC-8 | 24LC1025 | 2 |
| Y2 | Crystal oscillator | 16 MHz | 1 |
| Y1 | Crystal oscillator | 32.768 kHz | 1 |
| BT1 | Battery holder | — | 1 |
| D1, D2 | LED, 5 mm | — | 2 |
| R1, R2 | Resistor, I2C pull-up (RTC) | 10k Ω | 2 |
| R3, R4 | Resistor, I2C pull-up (EEPROM) | 4.7k Ω | 2 |
| R5, R7 | Resistor, LED current-limiting | 330 Ω | 2 |
| R6 | Resistor, reset pull-up | 10k Ω | 1 |
| C1, C4 | Decoupling capacitor | 0.1 µF | 2 |
| C2, C3 | Crystal load capacitor | 22 pF | 2 |
| C5 | Decoupling capacitor | 100 nF | 1 |
| J1 | I2C header | Conn_01×04_Pin | 1 |
| J2 | GPIO header | Conn_01×09_Pin | 1 |
| J3 | Serial UART header | Conn_01×04_Pin | 1 |
| J4 | ICSP header | Conn_02×03_Odd_Even | 1 |
| H1–H4 | Mounting holes | — | 4 |

---

## File Structure

```
├── kicad/
│   ├── MCU_Datalogger.kicad_pro        # KiCad project file
│   ├── MCU_Datalogger.kicad_sch        # Main schematic (sheet 1)
│   ├── Connectors.kicad_sch            # Connectors sub-sheet (sheet 2)
│   ├── MCU_Datalogger.kicad_pcb        # PCB layout
│   └── fp-lib-table                    # Footprint library references
├── assets/
│   ├── schematic_main.pdf              # Schematic sheet 1 export
│   ├── schematic_connectors.pdf        # Schematic sheet 2 export
│   ├── pcb_layout.png                  # PCB layout screenshot
│   ├── 3d_front.png                    # 3D render, front side
│   └── 3d_back.png                     # 3D render, back side
├── LICENSE
└── README.md
```

---

## License

This project is licensed under the [MIT License](LICENSE).
