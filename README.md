# MSP430G2553 Digital Thermometer — Complete Engineering Framework
**Project:** Digital Thermometer | MSP430G2553 + 16×2 LCD  
**Author:** Asiedu Seth Osei | KNUST Computer Engineering  
**Document Type:** Industry-Standard Engineering Reference  
**Version:** 1.0.0  

---

## TABLE OF CONTENTS

1. [GitHub Repository Structural Template](#1-github-repository-structural-template)
2. [System Architecture & Block Diagram](#2-system-architecture--block-diagram)
3. [Repository Keywords, Tags & Bill of Materials](#3-repository-keywords-tags--bill-of-materials)
4. [Schematic Design Checklist & Reference Connections](#4-schematic-design-checklist--reference-connections)
5. [Datasheet Reading & Compliance Guide](#5-datasheet-reading--compliance-guide)

---

## 1. GITHUB REPOSITORY STRUCTURAL TEMPLATE

### 1.1 Production-Grade Directory Tree

```
MSP430-Digital-Thermometer/
│
├── README.md                          # Project overview (template below)
├── LICENSE                            # MIT / BSD-3 recommended
├── .gitignore                         # CCS, IAR, Energia build artifacts
├── CHANGELOG.md                       # Version history
│
├── docs/                              # All project documentation
│   ├── datasheets/
│   │   ├── MSP430G2553_datasheet.pdf  # TI SLAS735 — DO NOT COMMIT (link in README)
│   │   └── HD44780_LCD_datasheet.pdf  # Standard LCD controller
│   ├── design/
│   │   ├── system_architecture.md     # This document / hardware decisions
│   │   ├── block_diagram.md           # ASCII block diagram (see Section 2)
│   │   └── design_decisions.md        # Why each component was chosen
│   ├── bom/
│   │   └── BOM_v1.0.csv               # Itemized Bill of Materials
│   └── testing/
│       ├── test_plan.md               # Functional verification checklist
│       └── test_results.md            # Logged measurements from hardware
│
├── hardware/                          # Schematic & PCB files
│   ├── schematic/
│   │   ├── digital_thermometer.kicad_sch
│   │   ├── digital_thermometer.kicad_pro
│   │   └── exports/
│   │       ├── schematic_v1.0.pdf     # Exported PDF for review
│   │       └── netlist_v1.0.net
│   ├── pcb/                           # Reserved for Phase 2 PCB layout
│   │   └── .gitkeep
│   └── symbols_footprints/            # Custom KiCad symbols if needed
│       └── .gitkeep
│
├── firmware/                          # All MCU source code
│   ├── src/
│   │   ├── main.c                     # Entry point, system init
│   │   ├── adc.c / adc.h              # ADC10 driver (internal sensor)
│   │   ├── lcd.c / lcd.h              # HD44780 LCD driver (4-bit mode)
│   │   ├── temp_sensor.c / .h         # Sensor abstraction layer
│   │   └── utils.c / utils.h          # Delay, math helpers
│   ├── include/
│   │   └── msp430g2553_config.h       # Pin definitions, clock config
│   ├── build/                         # .gitignored compiled output
│   └── tests/                         # Unit tests (simulatable)
│       └── test_adc_conversion.c
│
├── simulation/                        # Simulation files
│   ├── proteus/                       # Proteus ISIS project
│   │   └── digital_thermometer.pdsprj
│   └── wokwi/                         # Wokwi browser simulation
│       ├── diagram.json
│       └── wokwi.toml
│
└── assets/                            # Images for README and docs
    ├── block_diagram.png
    ├── schematic_preview.png
    └── demo.gif
```

> **Note on datasheets:** Never commit large PDFs to Git. Link to official TI and Hitachi/Winstar pages in your README instead.

---

### 1.2 README.md Boilerplate Template

```markdown
# 🌡️ MSP430G2553 Digital Thermometer

> A low-power embedded temperature measurement system built on the Texas Instruments 
> MSP430G2553 microcontroller, displaying real-time temperature on a 16×2 HD44780 LCD.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![MCU](https://img.shields.io/badge/MCU-MSP430G2553-red)](https://www.ti.com/product/MSP430G2553)
[![IDE](https://img.shields.io/badge/IDE-Code_Composer_Studio-green)](https://www.ti.com/tool/CCSTUDIO)
[![Status](https://img.shields.io/badge/Status-Phase_1_Internal_Sensor-yellow)]()

---

## 📋 Overview

This project implements a standalone digital thermometer with two planned phases:

| Phase | Sensor | Status |
|-------|--------|--------|
| 1 | MSP430G2553 Internal Temp Sensor (ADC10) | ✅ Active |
| 2 | External LM35 (Analog) or DS18B20 (1-Wire) | 🔲 Planned |

**Key Features:**
- Uses the MSP430G2553's built-in ADC10 and internal temperature diode
- 16×2 LCD output in 4-bit mode (conserves 4 GPIO pins)
- Sub-1mA standby power via LPM0
- Modular sensor abstraction layer for seamless Phase 2 upgrade
- All register-level code — no MSP430 DriverLib dependency

---

## 🔧 Hardware

| Component | Part Number | Notes |
|-----------|-------------|-------|
| MCU | MSP430G2553 (PDIP-20) | TI LaunchPad or breadboard |
| LCD | 16×2 HD44780 | 5V display, 4-bit mode |
| Contrast Pot | 10kΩ trimmer | LCD pin 3 (Vo) |
| Decoupling Caps | 100nF, 10µF | VCC bypass |
| Pull-up Resistor | 47kΩ | RST pin (pin 16) |
| Regulator (opt) | LM3940 3.3V | If powering from 5V USB |

See full [Bill of Materials](docs/bom/BOM_v1.0.csv).

---

## 🗂️ Repository Structure

```
See docs/design/system_architecture.md for the full directory layout.
```

---

## ⚡ Quick Start

### Prerequisites
- [Code Composer Studio](https://www.ti.com/tool/CCSTUDIO) v12+
- MSP430 GCC Toolchain
- MSP-EXP430G2ET LaunchPad (or standalone PDIP-20 + programmer)

### Build & Flash
```bash
# Clone the repo
git clone https://github.com/asieducodes/MSP430-Digital-Thermometer.git
cd MSP430-Digital-Thermometer/firmware

# Open in CCS: File > Import > CCS Project
# Build: Project > Build All (Ctrl+B)
# Flash: Run > Debug (F11)
```

---

## 📐 System Block Diagram

See [docs/design/block_diagram.md](docs/design/block_diagram.md) for the full ASCII hardware diagram.

---

## 📚 References

- [MSP430G2553 Datasheet (SLAS735)](https://www.ti.com/lit/ds/symlink/msp430g2553.pdf)
- [MSP430x2xx Family User's Guide (SLAU144)](https://www.ti.com/lit/ug/slau144k/slau144k.pdf)
- [HD44780 LCD Controller Datasheet](https://www.sparkfun.com/datasheets/LCD/HD44780.pdf)

---

## 📄 License

MIT — see [LICENSE](LICENSE) for details.
```

---

## 2. SYSTEM ARCHITECTURE & BLOCK DIAGRAM

### 2.1 Engineering Architecture Narrative

The system follows the canonical embedded hardware chain:

```
POWER INPUT → REGULATION → PROCESSING → SENSING → DISPLAY/ACTUATION
```

**Power Stage:** USB 5V (or 2×AA batteries ~3V) enters the system. For USB-powered designs, an LM3940 regulator steps 5V down to 3.3V for the MSP430. Battery operation can directly power the MSP430 (2.2V–3.6V operating range) with no regulator needed — a major advantage of the MSP430 family for portable designs.

**Processing Stage:** The MSP430G2553 runs at 1MHz from its internal DCO (Digitally Controlled Oscillator) by default, calibrated at factory to ±1%. The ADC10 module samples the internal temperature diode on Channel A10, converts the raw count to Celsius using a linear transfer function from the datasheet, and formats the string for display.

**Sensing Stage (Phase 1):** The internal temperature sensor is a diode whose forward voltage decreases with temperature (~−1.75 mV/°C, typical). The ADC10 references this against VREF+ (2.5V internal bandgap) and produces a 10-bit count (0–1023). Accuracy is ±2–3°C typical — sufficient for demonstration but not medical use.

**Sensing Stage (Phase 2 — modular upgrade path):**
- **LM35:** Analog output (10 mV/°C), connects to any ADC channel (A0–A7). No code restructuring needed — swap the ADC channel and adjust the conversion formula.
- **DS18B20:** 1-Wire digital protocol. Requires a new 1-Wire driver module, but the `temp_sensor.h` abstraction layer hides this from `main.c`.

**Display Stage:** A 16×2 HD44780-compatible LCD is driven in 4-bit mode (DB4–DB7 only). This halves GPIO consumption from 8 data lines to 4, at the cost of two write cycles per byte — an acceptable tradeoff at 1MHz and human-visible refresh rates.

---

### 2.2 Full ASCII System Block Diagram

```
╔═══════════════════════════════════════════════════════════════════════════╗
║                  MSP430G2553 DIGITAL THERMOMETER — SYSTEM ARCHITECTURE   ║
╚═══════════════════════════════════════════════════════════════════════════╝

  ┌────────────────────────────────────────────────────────────────────┐
  │                      POWER SUBSYSTEM                               │
  │                                                                    │
  │  USB 5V ──┬──► [ LM3940 3.3V REG ] ──► VCC_MCU (3.3V)            │
  │           │         │                        │                     │
  │           │    [10µF bulk]               [100nF]                  │
  │           │    [100nF bypass]             bypass                  │
  │           │                                  │                     │
  │           └──────────────────────────► VCC_LCD (5V)               │
  │                                        (LCD uses 5V logic)         │
  └────────────────────────────────────────────────────────────────────┘
                         │ 3.3V                      │ 5V
                         ▼                           ▼
  ┌──────────────────────────────────┐    ┌──────────────────────────┐
  │                                  │    │   DISPLAY SUBSYSTEM      │
  │      MSP430G2553 (PDIP-20)       │    │                          │
  │                                  │    │  16×2 HD44780 LCD        │
  │  ┌──────────────────────────┐    │    │                          │
  │  │  INTERNAL TEMP SENSOR    │    │    │  Pin 1 (VSS) → GND       │
  │  │  Channel A10             │    │    │  Pin 2 (VDD) → 5V        │
  │  │  (diode ≈ 986mV @ 25°C) │    │    │  Pin 3 (Vo)  → 10kΩ POT │
  │  └──────────┬───────────────┘    │    │  Pin 4 (RS)  ◄─── P1.0  │
  │             │ (internal wire)     │    │  Pin 5 (R/W) → GND      │
  │             ▼                    │    │  Pin 6 (EN)  ◄─── P1.1  │
  │  ┌──────────────────────────┐    │    │  Pin 7-10    → NC        │
  │  │  ADC10 MODULE            │    │    │  Pin 11(DB4) ◄─── P1.2  │
  │  │  10-bit SAR ADC          │    │    │  Pin 12(DB5) ◄─── P1.3  │
  │  │  Ref: VREF+ = 2.5V       │◄───│────│  Pin 13(DB6) ◄─── P1.4  │
  │  │  SREF = 1 (VR+)          │    │    │  Pin 14(DB7) ◄─── P1.5  │
  │  │  Sample Time: ≥30µs      │    │    │  Pin 15 (A+) → 5V (BL)  │
  │  └──────────┬───────────────┘    │    │  Pin 16 (A-) → GND (BL) │
  │             │ 10-bit raw count   │    └──────────────────────────┘
  │             ▼                    │          ▲ 4-bit GPIO data bus
  │  ┌──────────────────────────┐    │          │ (DB4-DB7, RS, EN)
  │  │  CPU / FIRMWARE          │    │──────────┘
  │  │  DCO @ 1MHz (BCSCTL)     │    │
  │  │  Temp = (V_sense - 986)  │    │
  │  │         / -1.75  + 25    │    │   ┌──────────────────────────┐
  │  │  (or use TI cal consts)  │────│──►│ PHASE 2: EXTERNAL SENSOR │
  │  └──────────────────────────┘    │   │                          │
  │                                  │   │  LM35 → P1.7 / A7 (ADC) │
  │  ┌──────────────────────────┐    │   │   OR                     │
  │  │  RESET & DEBUG           │    │   │  DS18B20 → P2.0 (GPIO)   │
  │  │  RST (Pin 16) → 47kΩ    │    │   │  with 4.7kΩ pull-up      │
  │  │  to VCC + 100nF to GND   │    │   └──────────────────────────┘
  │  │  TEST (Pin 17) → GND     │    │
  │  │  SBWTDIO (Pin 16)        │    │
  │  │  SBWTCK (Pin 17)         │    │
  │  └──────────────────────────┘    │
  │                                  │
  └──────────────────────────────────┘

LEGEND:
  ──► : Power flow
  ◄── : Control/data signal (MCU drives LCD)
  VCC_MCU = 3.3V (from LM3940)
  VCC_LCD = 5V  (direct from USB — HD44780 is 5V device)
  ⚠️  GPIO output at 3.3V driving 5V LCD inputs: check LCD V_IH spec.
      Most HD44780 clones accept V_IH ≥ 2.0V → 3.3V GPIO is sufficient.
      If uncertain, use a logic level shifter or 3.3V LCD module.
```

---

### 2.3 Data Flow Diagram

```
[Internal Temp Diode]
        │
        │ Analog voltage (~0.8–1.0V)
        ▼
[ADC10 — 10-bit SAR]
        │
        │ Raw 10-bit count (0–1023)
        ▼
[Conversion Formula in CPU]
  T(°C) = (ADC_count × VREF / 1023 − V_OFFSET) / TEMP_COEFF
        │
        │ Float/Integer temperature value
        ▼
[String Formatter]  "TEMP:  24.5 C"
        │
        │ ASCII character codes
        ▼
[HD44780 LCD Driver — 4-bit mode]
  RS=1, EN pulse, DB7–DB4 nibble HIGH, then nibble LOW
        │
        ▼
[16×2 LCD Display — User reads temperature]
```

---

## 3. REPOSITORY KEYWORDS, TAGS & BILL OF MATERIALS

### 3.1 GitHub Repository Tags & Keywords

Add these as **Topics** on your GitHub repository page (Settings → Topics):

```
msp430  msp430g2553  ti-launchpad  embedded-systems  microcontroller
digital-thermometer  temperature-sensor  adc  lcd-display  hd44780
c-language  bare-metal  low-power  msp430-launchpad  iot
code-composer-studio  kicad  electronics  student-project  knust
internal-temperature-sensor  lm35  ds18b20  16x2-lcd  4-bit-lcd
```

**For your README description:**  
> "Bare-metal digital thermometer on MSP430G2553 using ADC10 internal sensor and HD44780 16×2 LCD. Register-level C with modular architecture for LM35/DS18B20 upgrade."

---

### 3.2 Bill of Materials (BOM) Checklist

#### SECTION A — Core MCU System Components

| ☐ | Ref | Component | Part Number | Package | Value/Spec | Qty | Notes |
|---|-----|-----------|-------------|---------|------------|-----|-------|
| ☐ | U1 | Microcontroller | MSP430G2553IPW20 | PDIP-20 | 16MHz max, 3.3V | 1 | Main CPU |
| ☐ | C1 | Decoupling Cap | Generic | 0402 / TH | 100nF, 16V | 1 | VCC pin bypass, place ≤5mm from pin |
| ☐ | C2 | Bulk Cap | Electrolytic | TH | 10µF, 10V | 1 | Power rail stability |
| ☐ | R1 | RST Pull-up | Generic | TH | 47kΩ, 1/4W | 1 | RST pin 16 to VCC |
| ☐ | C3 | RST Debounce | Generic | 0402 / TH | 100nF | 1 | RST pin 16 to GND |
| ☐ | J1 | Programming Header | 2×5 pin 0.05" | Through-hole | SBW2 (2-pin) | 1 | SBWTDIO + SBWTCK |
| ☐ | SW1 | Reset Button | Tactile SPST | TH | 6mm | 1 | Optional manual reset |
| ☐ | — | Solderless Breadboard | — | — | 830-point | 1 | Prototyping |
| ☐ | — | Jumper Wires | — | M-M | 20cm | 20 | Assorted |

#### SECTION B — Power Management Subsystem

| ☐ | Ref | Component | Part Number | Package | Value/Spec | Qty | Notes |
|---|-----|-----------|-------------|---------|------------|-----|-------|
| ☐ | U2 | LDO Regulator | LM3940IT-3.3 | TO-220 | 5V→3.3V, 1A max | 1 | If powering MCU from 5V USB |
| ☐ | C4 | Reg Input Cap | Electrolytic | TH | 10µF, 16V | 1 | LM3940 V_IN bypass |
| ☐ | C5 | Reg Output Cap | Electrolytic | TH | 22µF, 10V | 1 | LM3940 V_OUT stability (≥10µF required) |
| ☐ | J2 | USB Type-B / Micro | Generic | TH | 5V supply | 1 | Power input connector |
| ☐ | F1 | Polyfuse | Littelfuse 0.5A | TH | 500mA resettable | 1 | Overcurrent protection |
| ☐ | D1 | Reverse Polarity | 1N4007 | DO-41 | 1A, 1000V | 1 | Series diode on V_IN |
| ☐ | — | 2× AA Battery Holder | — | TH | 3V (~2.4–3.2V) | 1 | Alt: battery-only design |

> **Battery Design Note:** MSP430G2553 operates down to 2.2V. Two alkaline AAs (3V fresh) eliminate the need for a regulator entirely — this is the classic low-power MSP430 use case.

#### SECTION C — Display and User Interface

| ☐ | Ref | Component | Part Number | Package | Value/Spec | Qty | Notes |
|---|-----|-----------|-------------|---------|------------|-----|-------|
| ☐ | LCD1 | 16×2 Character LCD | JHD162A / Generic | TH 16-pin DIP | HD44780 compatible | 1 | 5V logic preferred |
| ☐ | RV1 | Contrast Potentiometer | Bourns 3386P | TH trimmer | 10kΩ | 1 | LCD pin 3 (Vo) |
| ☐ | R2 | Backlight Resistor | Generic | TH | 100Ω–220Ω | 1 | LCD pin 15 (LED+) limiter |
| ☐ | — | 16-pin Header Strip | Generic | 2.54mm pitch | Male, right-angle | 1 | LCD connector |

#### SECTION D — Sensor Hardware (Internal + Future External)

| ☐ | Ref | Component | Part Number | Package | Value/Spec | Qty | Phase | Notes |
|---|-----|-----------|-------------|---------|------------|-----|-------|-------|
| ☐ | — | Internal Sensor | MSP430G2553 A10 | On-die | −55 to 125°C, ±2°C | — | 1 | No external component needed |
| ☐ | U3 | LM35 Temp Sensor | LM35DZ | TO-92 | 10mV/°C, 2–35V | 1 | 2 | Analog; wire to A7 (P1.7) |
| ☐ | C6 | LM35 Bypass | Generic | TH | 100nF | 1 | 2 | At LM35 VCC pin |
| ☐ | U4 | DS18B20 Sensor | DS18B20 | TO-92 | 1-Wire, 3–5.5V | 1 | 2 | Digital; wire to P2.0 |
| ☐ | R3 | DS18B20 Pull-up | Generic | TH | 4.7kΩ | 1 | 2 | 1-Wire data line to VCC |
| ☐ | J3 | Sensor Header | 3-pin 2.54mm | TH | Male | 1 | 2 | VCC, DATA, GND external connector |

---

## 4. SCHEMATIC DESIGN CHECKLIST & REFERENCE CONNECTIONS

### 4.1 MSP430G2553 Core Power & Reset Network

```
POWER & DECOUPLING CHECKLIST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
☐  VCC  (Pin 1)  → 3.3V rail
☐  GND  (Pin 20) → GND plane
☐  100nF cap from Pin 1 to Pin 20 (as close to pins as possible)
☐  10µF electrolytic from VCC rail to GND (at power entry point)
☐  RST  (Pin 16) → 47kΩ pull-up to VCC
☐  RST  (Pin 16) → 100nF to GND  (filters voltage dips during supply ramp-up)
☐  TEST (Pin 17) → GND (prevents spurious Spy-Bi-Wire entry in standalone use)
☐  DVCC (Pin 1)  = AVCC: single VCC rail, no split supply on G2553
☐  DVSS (Pin 20) = AVSS: single GND, no AGND separate plane needed

PROGRAMMING / DEBUG HEADER (Spy-Bi-Wire)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
☐  SBWTDIO → RST (Pin 16)   [shared with RST function]
☐  SBWTCK  → TEST (Pin 17)  [shared with TEST function]
☐  2-pin 0.1" header to MSP-EXP430G2ET LaunchPad programmer port
☐  Series 100Ω resistors on SBWTDIO / SBWTCK lines (protects programmer)
```

---

### 4.2 MSP430G2553 GPIO → HD44780 LCD Pin Mapping (4-bit mode)

#### Complete Pin-to-Pin Reference Table

```
┌────────────────────────────────────────────────────────────────────────┐
│           MSP430G2553 ↔ HD44780 LCD — 4-BIT MODE CONNECTION MAP        │
├──────────────┬───────────┬───────────────┬──────────────────────────────┤
│ LCD Signal   │ LCD Pin # │ MSP430 Pin    │ Notes                        │
├──────────────┼───────────┼───────────────┼──────────────────────────────┤
│ VSS (GND)    │ 1         │ GND           │ LCD ground                   │
│ VDD (+5V)    │ 2         │ 5V rail       │ LCD logic power              │
│ Vo (Contrast)│ 3         │ RV1 wiper     │ 10kΩ pot: GND ←→→ 5V        │
│ RS           │ 4         │ P1.0 (Pin 2)  │ Register Select: 0=Cmd,1=Data│
│ R/W          │ 5         │ GND           │ Tie LOW = Write-only mode     │
│ EN           │ 6         │ P1.1 (Pin 3)  │ Enable clock: HIGH→LOW latch │
│ DB0          │ 7         │ — (NC)        │ Not used in 4-bit mode        │
│ DB1          │ 8         │ — (NC)        │ Not used in 4-bit mode        │
│ DB2          │ 9         │ — (NC)        │ Not used in 4-bit mode        │
│ DB3          │ 10        │ — (NC)        │ Not used in 4-bit mode        │
│ DB4          │ 11        │ P1.2 (Pin 4)  │ Data bit 4 (LSB of nibble)   │
│ DB5          │ 12        │ P1.3 (Pin 5)  │ Data bit 5                   │
│ DB6          │ 13        │ P1.4 (Pin 6)  │ Data bit 6                   │
│ DB7          │ 14        │ P1.5 (Pin 7)  │ Data bit 7 (MSB of nibble)   │
│ LED+ (BL+)   │ 15        │ 5V via 100Ω   │ Backlight anode (always on)  │
│ LED- (BL-)   │ 16        │ GND           │ Backlight cathode            │
└──────────────┴───────────┴───────────────┴──────────────────────────────┘

MSP430G2553 PDIP-20 PINOUT REFERENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Pin  1 : DVCC          → 3.3V
Pin  2 : P1.0 / TA0CLK → RS  (LCD Register Select)
Pin  3 : P1.1 / TA0.0  → EN  (LCD Enable)
Pin  4 : P1.2 / TA0.1  → DB4 (LCD data)
Pin  5 : P1.3 / ADC A3  → DB5 (LCD data)
Pin  6 : P1.4 / SMCLK   → DB6 (LCD data)
Pin  7 : P1.5 / TA0.0   → DB7 (LCD data)
Pin  8 : P1.6 / TA0.1   → [FREE — LED indicator optional]
Pin  9 : P1.7 / ADC A7  → [RESERVED for Phase 2: LM35 analog input]
Pin 10 : P2.0 / TA1.0   → [RESERVED for Phase 2: DS18B20 1-Wire]
Pin 11 : P2.1 / TA1.1   → [FREE]
Pin 12 : P2.2 / TA1.1   → [FREE]
Pin 13 : P2.3 / TA1.0   → [FREE]
Pin 14 : P2.4 / TA1.2   → [FREE]
Pin 15 : P2.5 / TA1.2   → [FREE]
Pin 16 : RST / SBWTDIO  → 47kΩ→VCC, 100nF→GND, programmer
Pin 17 : TEST / SBWTCK  → GND (standalone) / programmer
Pin 18 : XOUT / P2.7    → [NC — no crystal used]
Pin 19 : XIN / P2.6     → [NC — using internal DCO]
Pin 20 : DVSS           → GND
```

> **Signal Integrity Note on R/W (LCD Pin 5):** Tying R/W permanently to GND forces write-only mode. This eliminates the need to read the LCD's busy flag (BF), which would require bidirectional GPIO toggling. Instead, use fixed delay loops calibrated to the HD44780's worst-case timing (see Section 5). This is the universally recommended approach for MCU-to-LCD interfacing.

---

### 4.3 Schematic Capture Checklist (KiCad / Altium)

```
PRE-CAPTURE
━━━━━━━━━━━
☐  Download MSP430G2553 symbol from TI KiCad library or Texas Instruments KiCad library
☐  Download HD44780 16×2 LCD symbol or create custom 16-pin component
☐  Create project in KiCad 7+ (digital_thermometer.kicad_pro)
☐  Set schematic grid to 50mil (standard)

MCU PLACEMENT & CONNECTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
☐  Place U1 (MSP430G2553) center of sheet
☐  Connect VCC (Pin 1) to power net "+3V3"
☐  Connect GND (Pin 20) to power net "GND"
☐  Place C1 (100nF) between Pin 1 and Pin 20 — label as "VCC bypass"
☐  Place C2 (10µF) on +3V3 net near power entry
☐  Connect Pin 16 (RST) to:
    → R1 (47kΩ) top → +3V3
    → C3 (100nF) top → GND
    → SW1 (reset button) → GND [optional]
    → J1 Pin 1 (SBWTDIO)
☐  Connect Pin 17 (TEST) to GND and J1 Pin 2 (SBWTCK)
☐  Mark Pins 18, 19 (XIN/XOUT) as NC with no-connect markers

LCD CONNECTIONS
━━━━━━━━━━━━━━━
☐  Place LCD1 (16×2) on sheet
☐  Pin 1 (VSS) → GND
☐  Pin 2 (VDD) → +5V power net
☐  Pin 3 (Vo) → RV1 wiper; RV1 ends → +5V and GND
☐  Pin 4 (RS) → U1 P1.0 (Pin 2)
☐  Pin 5 (R/W) → GND  [tie low, add label "WRITE-ONLY"]
☐  Pin 6 (EN) → U1 P1.1 (Pin 3)
☐  Pins 7–10 (DB0–DB3) → NC markers
☐  Pin 11 (DB4) → U1 P1.2 (Pin 4)
☐  Pin 12 (DB5) → U1 P1.3 (Pin 5)
☐  Pin 13 (DB6) → U1 P1.4 (Pin 6)
☐  Pin 14 (DB7) → U1 P1.5 (Pin 7)
☐  Pin 15 (LED+) → R2 (100Ω) → +5V
☐  Pin 16 (LED-) → GND

POWER SUBSYSTEM (if USB-powered)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
☐  Place J2 (USB connector) — only need VBUS (+5V) and GND
☐  Place D1 (1N4007) in series with VBUS for reverse polarity
☐  Place F1 (polyfuse) after D1
☐  Place U2 (LM3940): IN→5V, OUT→+3V3, ADJ→GND (fixed 3.3V variant)
☐  C4 (10µF) on LM3940 IN pin
☐  C5 (22µF) on LM3940 OUT pin [LM3940 is unstable without this cap!]

PHASE 2 FORWARD-COMPATIBLE HEADERS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
☐  Place J3 (3-pin header) labeled "EXT_SENSOR"
    Pin 1 → VCC (+3.3V or +5V solder jumper)
    Pin 2 → P1.7 (A7 ADC) [LM35] or P2.0 [DS18B20]
    Pin 3 → GND
☐  Add DNP (Do Not Populate) note for Phase 2 components

DESIGN RULE CHECKS
━━━━━━━━━━━━━━━━━━
☐  Run ERC — zero errors expected
☐  All power pins connected
☐  No unconnected pins without explicit NC markers
☐  All decoupling caps within 5mm of their associated VCC pin
☐  Export PDF schematic to docs/hardware/exports/
```

---

## 5. DATASHEET READING & COMPLIANCE GUIDE

### 5.1 MSP430G2553 Datasheet Parameters to Verify

**Document:** SLAS735J — *MSP430G2x53, MSP430G2x13 Mixed Signal Microcontroller*  
**URL:** https://www.ti.com/lit/ds/symlink/msp430g2553.pdf  
Also required: **SLAU144K** — *MSP430x2xx Family User's Guide* (for module-level detail)

---

#### 5.1.1 Absolute Maximum Ratings (Table 1 of SLAS735J)

| Parameter | Datasheet Symbol | Value to Look Up | Why It Matters for Your Design |
|-----------|-----------------|------------------|-------------------------------|
| Supply voltage | V_CC | −0.3V to +3.9V | **Never exceed 3.9V on VCC.** If using a 3.3V LDO, ensure regulator worst-case output + tolerance stays below this. LM3940 output accuracy is ±2% → 3.366V max. ✅ Safe. |
| Voltage on any I/O pin | V_I | −0.3V to V_CC +0.3V | **Your 5V LCD with 3.3V GPIO:** GPIO drives 3.3V signals into 5V LCD inputs. Since MSP430 drives (not receives) the LCD, this is output-only — no 5V signal enters an MSP430 pin. ✅ Safe. But never connect a 5V signal source to an MSP430 GPIO input without a level shifter or voltage divider. |
| Current into/out of any I/O pin | I_IO | ±25mA per pin | LCD data lines draw essentially no current (CMOS inputs). Backlight LED current (20–80mA) must **never** flow through a GPIO pin — always use a series resistor from VCC, not from GPIO. ✅ Your design routes backlight through R2 from 5V. |
| Total device current (all ports) | I_total | ±50mA (check Table 1) | Sum of all active GPIO current sinks/sources must not exceed this. |

---

#### 5.1.2 Recommended Operating Conditions

| Parameter | Min | Typ | Max | Your Design Decision |
|-----------|-----|-----|-----|----------------------|
| V_CC supply | 2.2V | 3.0V | 3.6V | LM3940 at 3.3V — within range ✅ |
| Operating temperature | −40°C | 25°C | 85°C | Benchtop/indoor use ✅ |
| DCO frequency at 1MHz calibration | — | 1.000MHz | — | Uses CALBC1_1MHZ + CALDCO_1MHZ from Info Flash |
| Crystal (if used) | — | — | 16MHz | Not used; DCO preferred for cost savings |

---

#### 5.1.3 ADC10 Operating Conditions — Critical Parameters

**Location in datasheet:** Electrical Characteristics → ADC10 Characteristics section

| Parameter | Symbol | Value | Impact on Your Firmware |
|-----------|--------|-------|------------------------|
| ADC10 reference voltage (internal) | V_REF+ | 2.500V typical (±0.1V) | Your conversion formula must use this value: `V_sense = (ADC_count / 1023.0) × 2.5` |
| ADC10 clock source | ADC10CLK | ADC10OSC ~5MHz | Each sample-and-hold clock is 1 ADC10CLK cycle. At 5MHz, 1 tick = 200ns |
| Sample-and-hold time | SHT | 4, 8, 16, 64, 128, 256, 384, 768 ADC10CLK | You must set SHT to meet the internal sensor requirement (see below) |
| ADC10 input impedance requirement | — | Source resistance ≤ 50kΩ | Internal sensor impedance is much lower ✅ |
| ADC10 reference settling time | — | ≥ 17µs after enabling | Add `__delay_cycles(20)` at 1MHz = 20µs after turning on REFON before sampling |
| DNL (Differential Non-Linearity) | DNL | ±1 LSB typ | 10-bit ADC has 1024 steps over 2.5V → 1 step ≈ 2.44mV. At 1.75mV/°C sensor sensitivity, 1 LSB ≈ 1.4°C resolution — acceptable for Phase 1 |

---

#### 5.1.4 Internal Temperature Sensor — Most Critical Parameters

**Location:** Electrical Characteristics → Temperature Sensor (Section 12 in SLAU144)

```
CRITICAL PARAMETERS TO EXTRACT FROM SLAS735J:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Parameter 1: V_TEMP (sensor voltage at 25°C)
  → Typical value: ~986 mV (varies ±50mV device-to-device)
  → Used in conversion formula as V_OFFSET

Parameter 2: Temperature coefficient (TC_TEMP)
  → Typical: −1.75 mV/°C (negative — voltage DECREASES as temp rises)
  → Used as TEMP_COEFF in formula

Parameter 3: Minimum sample time
  → The internal temperature sensor requires a MINIMUM sample time
  → Look up: "temperature sensor sample time" in the ADC10 section
  → Typical value: ≥ 30 µs
  → At ADC10OSC ≈ 5MHz: 30µs × 5MHz = 150 clock cycles
  → Therefore: SHT must be set to at least 64 ADC10CLK ticks (64 × 200ns = 12.8µs)
     For safety, use SHT = 3 (64 clocks) or SHT = 4 (128 clocks)

Parameter 4: Reference settling time
  → Wait ≥ 30µs after enabling REFON before first ADC sample
  → Use __delay_cycles(30) at 1MHz MCLK

Parameter 5: Factory calibration constants (from Info Flash)
  → TI stores two-point calibration data in Information Flash (Segment A):
    CAL_ADC_25T85 — ADC counts at 85°C
    CAL_ADC_25T30 — ADC counts at 30°C
  → These are DEVICE-SPECIFIC and more accurate than the typical datasheet values
  → Preferred formula using calibration constants:
    T_C = 30 + ((ADC_count - CAL_ADC_25T30) × (85-30)) / (CAL_ADC_25T85 - CAL_ADC_25T30)
```

---

#### 5.1.5 Clock & VCC Stability Recommendations

| Requirement | Datasheet Source | Your Implementation |
|-------------|-----------------|---------------------|
| DCO calibration must be applied before ADC use | SLAU144 §4.3 | Write `BCSCTL1 = CALBC1_1MHZ; DCOCTL = CALDCO_1MHZ;` at top of `main()` — always before any peripheral init |
| VCC must be stable during ADC sampling | SLAU144 §22.2 | The 100nF + 10µF decoupling network stabilizes VCC. Never run ADC conversions during I/O port switching storms — use LPM0 with ADC interrupt |
| ADC10 reference requires minimum startup time | SLAS735J ADC table | Enable REFON, wait 30µs, then trigger conversion. Never sample immediately after REFON |
| MCLK stability | SLAU144 §4.2 | Factory calibration at 1MHz provides ±1% accuracy. This is sufficient for LCD timing delays and ADC clock |

---

### 5.2 HD44780 LCD Controller Datasheet Parameters

**Document:** Hitachi HD44780U datasheet  
**Key Timing Parameters:**

| Parameter | Symbol | Min | Max | Your Firmware Implication |
|-----------|--------|-----|-----|--------------------------|
| Power-on initialization delay | t_por | 40ms | — | Add `__delay_cycles(40000)` at 1MHz after VCC stable. This is the most common bug: LCD not initializing because firmware runs before LCD is ready |
| Enable pulse width (high) | PW_EH | 450ns | — | At 1MHz MCU, each instruction = 1µs. A single GPIO write guarantees >450ns ✅ |
| Address setup time before EN↑ | t_AS | 140ns | — | Write RS/RW first, then toggle EN. At 1MHz this is automatically met ✅ |
| Data setup time before EN↓ | t_DSW | 195ns | — | Data (DB4–DB7) must be stable before EN goes LOW. Set GPIO data, then pulse EN ✅ |
| LCD command execution time | t_exec | 37µs | 1.52ms | After most commands: delay 37µs minimum. After CLEAR DISPLAY and CURSOR HOME: delay 1.52ms. Since R/W is tied low (no busy flag), always use delay |
| Return home / clear screen | — | — | 1.52ms | Always add 2ms delay after these two commands |
| 4-bit initialization sequence | — | — | — | Send nibble 0x3, wait 4.1ms; send 0x3 again, wait 100µs; send 0x3 third time, wait 100µs; THEN send 0x2 to switch to 4-bit mode |

---

### 5.3 How Datasheet Parameters Dictate Your Hardware & Software Choices

```
DESIGN DECISION TRACEABILITY TABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Decision: Use 100nF bypass capacitor on VCC pin
Datasheet source: SLAU144 §12.1, application note SLAA011
Why: ADC10 draws transient current during conversion (~400µA spike at 5MHz clock).
     Without bypass cap, VCC droops, causing ADC reference error.
     100nF at ≤5mm from VCC pin has ESL low enough to suppress this spike.
     10µF bulk cap handles slower supply fluctuations and LCD inrush.

Decision: 47kΩ RST pull-up (not 10kΩ or 100kΩ)
Datasheet source: SLAS735J §6 — RST/NMI pin specifications
Why: MSP430 RST input has a weak internal pull-up (~47kΩ typical).
     An external 47kΩ mirrors this, ensuring RST rises smoothly with VCC.
     Too low (1kΩ): wastes power and loads down programmer during SBW.
     Too high (1MΩ): RST rises too slowly, MCU may enter brownout during startup.

Decision: SHT = 64 ADC10CLK ticks for internal temperature sensor
Datasheet source: SLAS735J Table — Temp sensor sample time requirement ≥30µs
Calculation:
     ADC10CLK = ADC10OSC ≈ 5MHz → period = 200ns/clock
     Required: 30µs ÷ 200ns = 150 clock cycles minimum
     Available SHT values: 4, 8, 16, 64, 128...
     64 clocks × 200ns = 12.8µs → NOT sufficient!
     Use SHT = 4 (128 clocks) → 128 × 200ns = 25.6µs → still marginal
     Use SHT = 5 (256 clocks) → 256 × 200ns = 51.2µs ✅ SAFE
     In ADC10CTL0: SHT0_5 (SHT = 256 clocks)

Decision: 30µs REFON settling delay in firmware
Datasheet source: SLAS735J ADC10 timing table
Why: Internal 2.5V bandgap reference has a capacitive startup transient.
     Sampling before it settles means VREF+ is below 2.5V, causing
     all temperature readings to read artificially high.
     Implementation: REFON bit set → __delay_cycles(30) → then start conversion

Decision: 40ms LCD power-on delay in firmware
Datasheet source: HD44780 Application Note, §4 Initialization
Why: HD44780 controller runs its internal reset sequence when VCC crosses 4.5V.
     This takes 40ms. If your MCU issues commands before this,
     the LCD controller is still in reset and ignores all inputs.
     The LCD will appear blank — the #1 reported bug on embedded LCD projects.

Decision: 4.7kΩ pull-up for DS18B20 (Phase 2)
Datasheet source: DS18B20 Datasheet §4 — Absolute Maximum, Parasite Power
Why: DS18B20 uses open-drain 1-Wire. Pull-up must source the conversion current
     (~1.5mA peak during temperature conversion). Too high (10kΩ): signal
     rise time too slow, violates 1-Wire timing. Too low (1kΩ): exceeds
     DS18B20 V_IH and wastes power. 4.7kΩ is the standard value specified
     in the DS18B20 datasheet itself — always use the manufacturer's value.

Decision: 100Ω LCD backlight resistor
Calculation:
     LED forward voltage (typical): 3.3V (white LED backlight on 5V LCD)
     V_resistor = V_supply - V_forward = 5V - 3.3V = 1.7V
     Target LED current: 15mA (moderate brightness, within 20mA max)
     R = V / I = 1.7V / 0.015A = 113Ω → use 100Ω standard value
     Actual I = 1.7V / 100Ω = 17mA ✅ safe
```

---

### 5.4 Phase 2 Upgrade — Sensor Abstraction Architecture

To make the transition to an external sensor seamless, design the firmware with this interface:

```c
/* temp_sensor.h — Abstraction Layer */

typedef enum {
    SENSOR_INTERNAL,    // MSP430G2553 A10 — Phase 1
    SENSOR_LM35,        // Analog, A7 pin   — Phase 2a
    SENSOR_DS18B20      // 1-Wire, P2.0     — Phase 2b
} SensorType_t;

/* Call once at init */
void Sensor_Init(SensorType_t type);

/* Returns temperature in 0.1°C units (e.g., 245 = 24.5°C) */
int16_t Sensor_ReadTemp(void);
```

`main.c` only ever calls `Sensor_Init()` and `Sensor_ReadTemp()`. Switching from Phase 1 to Phase 2 requires changing **one line** in `main.c`:

```c
// Phase 1:
Sensor_Init(SENSOR_INTERNAL);

// Phase 2 (swap one line):
Sensor_Init(SENSOR_LM35);  // or SENSOR_DS18B20
```

All ADC channel changes, 1-Wire protocol, and conversion formulas live inside `temp_sensor.c` — invisible to `main.c`. This is the modular design principle that makes embedded systems maintainable.

---

## APPENDIX: QUICK REFERENCE CHECKLIST

```
PROJECT READINESS CHECKLIST — Before Powering On
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
☐  All decoupling capacitors placed
☐  RST pull-up resistor confirmed
☐  R/W LCD pin tied to GND
☐  Backlight resistor R2 in series (not LCD pin to GPIO!)
☐  Contrast potentiometer RV1 wired correctly (GND ↔ +5V, wiper → Vo)
☐  LM3940 has 22µF output capacitor (LM3940 datasheet requirement)
☐  Programmer header pins correct (SBWTDIO=RST, SBWTCK=TEST)
☐  No 5V signal connected to any MSP430 input pin
☐  ADC SHT set to 256 clocks (SHT0_5) for internal temp sensor
☐  40ms power-on delay in LCD init routine
☐  DCO calibration constants applied at start of main()
☐  REFON settling delay (30µs) before first ADC conversion
☐  P1.7 and P2.0 left as inputs (high impedance) in Phase 1 — not driven low
```

---

*End of Engineering Framework Document*  
*Generated for: MSP430G2553 Digital Thermometer Project*  
*Author: Asiedu Seth Osei | KNUST Computer Engineering | asieducodes*
