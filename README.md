# FCH7 - Flight Controller for Drone Quadcopter

##  Overview

FCH7 is a project aimed at developing a flight controller based on the STM32H743VITx MCU for drones with 4 to 8 motors, designed using KiCad.
The flight controller is in a 30x30mm form factor, with the goal of running it under Betaflight. It is for the moment only designed for DJI HD VTX.
> ** Project status:** This project is currently under development. The schematics are completed and the PCB is being routed.

##  Main Features

### Microcontroller
- **STM32H743VITx** - ARM Cortex-M7 dual core processor running at 480 MHz
  - 2048 KB Flash memory
  - 1024 KB RAM
  - 82 available GPIOs
  - LQFP100 package
  - 
### Embedded Sensors
- **ICM42688P** - Modern IMU (gyroscope + accelerometer)
- **MPU6500** - Secondary IMU for redundancy
- **QMC5883L** - 3-axis magnetometer for compass
- **SPL06-001** - Barometer for altitude

### Connectivity
- **ExpressLRS (ELRS)** - Integrated 2.4 GHz RF receiver
  - Dedicated RF_2.4 module
  - Support for long-range and low-latency link
- **USB-C** - 16-pin USB 2.0 connector for configuration and updates
- **HDGC1002WR Connectors** - Multiple connectors for peripherals
  - GPS (4 pins)
  - DJI HD VTX (6 pins)
  - Additional UART ports

### Power Supply
- **Intelligent power management**
  - **MP9943GQ-z** regulator - Main buck converter
  - **ME6217C33M5G** regulator - 3.3V LDO
  - Support pour batteries LiPo (VBAT)
  - Rails multiples: VDD_MCU, VDD_RECV, 4V5, VDD_GYRO

### Storage and LEDs
- **Blackbox** - SDIO interface for microSD card
  - Flight data logging for analysis
- **WS2812 LEDs** (addressable) - Programmable visual indicators
- **Status LEDs** - Multiple LEDs for diagnostics
##  Project Architecture

The project is organized into distinct hierarchical modules:

```
Folder Structure
C:.
│   .gitignore
│   fch7.kicad_pcb
│   fch7.kicad_prl
│   fch7.kicad_pro
│   fclib.bak
│   fclib.kicad_sym
│   fp-info-cache
│   fp-lib-table
│   README.md
│   sym-lib-table     
├───Hardware
│   ├───3D
│   │       CSTNE8M00G55Z000R0.stp
│   │       ICM-42688-P.stp
│   │       ME6217C33M5G.step
│   │       MP9943AGQ-Z.stp
│   │       MPU-6500.stp
│   │       STM32H743VIT6.stp
│   │
│   ├───Footprints
│   │   └───fch7_fp.pretty
│   │           AP2112K-1.8TRG1.kicad_mod
│   │           CONN-SMD_HDGC1002WR-S-6P.kicad_mod
│   │           CONN-SMD_HDGC1002WR-S-8P.kicad_mod
│   │           CRYSTAL-SMD_4P-L2.0-W1.6-BL.kicad_mod
│   │           CSNP1GCR01-BOW.kicad_mod
│   │           CSTNE8M00G55Z000R0.kicad_mod
│   │           IIM42352.kicad_mod
│   │           LED0603-R-RD_RED.kicad_mod
│   │           LGA-16_L3.0-W3.0-P0.50-BL_SQ.kicad_mod
│   │           LQFP-100_14x14mm_P0.5mm_STM32H743.kicad_mod
│   │           QFN40P300X300X95-25M-D.kicad_mod
│   │           RES-ARRAY-SMD_0603-8P-L3.2-W1.6-BL.kicad_mod
│   │           SENSOR-SMD_SPL06-001.kicad_mod
│   │           SM03B-SRSS-TB(LF)(SN).kicad_mod
│   │           SolderPad_1x01_SMD_2.2x1.6mm.kicad_mod
│   │           SON65P300X300X100-8N.kicad_mod
│   │           SOT23-5.kicad_mod
│   │           STM32H743VIT6.mod
│   │           SW-SMD_2P-TS24CA.kicad_mod
│   │           TSSOP-28_L9.7-W4.4-P0.65-LS6.4-BL-EP-2.kicad_mod
│   │           USB-C-SMD_TYPE-C-6PIN-2MD-073.kicad_mod
│   │           WIFIM-SMD_ESP-01F-2M.kicad_mod
│   │           WIRELM-SMD_E28-2G4M27SX.kicad_mod
│   └───Symbols
│       └───fch7_sym
│               CSTNE8M00G55Z000R0.bak
│               CSTNE8M00G55Z000R0.kicad_sym
│               fch7_sym.bak
│               fch7_sym.kicad_sym
│               ICM-42688-P.kicad_sym
│               ME6217C33M5G.bak
│               ME6217C33M5G.kicad_sym
│               MP9943AGQ-Z.bak
│               MP9943AGQ-Z.kicad_sym
│               MPU-6500.kicad_sym
│               STM32H743VIT6.kicad_sym
└───Schematic
        blackbox_led.kicad_sch
        connectors.kicad_sch
        ERC.rpt
        fch7.csv
        fch7.kicad_sch
        fp-info-cache
        MCU.kicad_sch
        Power.kicad_sch
        rf_rx.kicad_sch
        sensors.kicad_sch
        ~fch7.kicad_sch.lck
```

##  Custom Components

The project uses a custom symbol library (`fclib.kicad_sym`) containing:
- ICM42688P (IMU)
- MPU6500 (IMU)
- QMC5883L (Magnetometer)
- SPL06-001 (Barometer)
- elrs_rx (RF Modules)
- MP9943GQ-z (Buck converter)
- ME6217C33M5G (LDO)
- Connecteurs HDGC1002WR (4P, 6P, 8P)
- CSNP1GCR01-BOW
- WS2812_ (LEDS)

##  Applications

This flight controller is designed for:
- Freestyle drones
- Long-range drones
- Stabilized and acrobatic flight applications
- Compatibility with open-source firmwares (Betaflight)
- 
##  Technical Specifications

### Available Interfaces
- **SPI** - High-speed communication with sensors and OSD
- **I2C** - Communication with magnetometer and barometer
- **UART** - Multiple ports for GPS, telemetry, peripherals
- **SDIO** - SD card interface for blackbox
- **PWM/DShot** - Motor control (via MCU GPIO)
- **USB** - Configuration and firmware update

### Power Requirements
- Input: LiPo Batteries (2-6S typical)
- Regulated rails: 3.3V, 4.5V, VDD_MCU, VDD_RECV, VDD_GYRO
- Reverse polarity protection

##  Required Tools

### Software
- **KiCad 9.0** or higher - Schematic and PCB design
- **Git** - Version control
- 
##  Development Roadmap

- [x] MCU schematic
- [x] Power schematic
- [x] Sensors schematic
- [x] RF receiver schematic
- [x] Blackbox/LEDs schematic
- [x] Connectors schematic
- [x] Custom symbol library
- [x] PCB layout
- [ ] Track routing
- [ ] DRC/ERC verification
- [ ] Gerber file generation
- [ ] Prototype and testing
- [ ] Base firmware
- [ ] User documentation

##  Design Notes

##  Contribution

This project is under active development. Contributions, suggestions, and feedback are welcome !
##  Auteur

Maxim - [maks7d](https://github.com/maks7d)

### Communauté
- [ExpressLRS](https://www.expresslrs.org/) - ExpressLRS Documentation
- [KiCad Forum](https://forum.kicad.info/) - KiCad Support

---

**Version:** 0.1-alpha  
**Last Updated:** December 2025