# Low-Cost M-Class Phasor Measurement Unit (PMU)

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Standard](https://img.shields.io/badge/IEEE-C37.118-green.svg)
![Hardware](https://img.shields.io/badge/Hardware-STM32%20%7C%20ADE7880-orange)

## Project Overview
This repository contains the complete hardware design, firmware, and software implementation of a **Low-Cost M-Class Phasor Measurement Unit (PMU)**. 

Designed for smart grid research and educational purposes, this device measures voltage and current phasors, frequency, and power metrics in real-time. It synchronizes measurements using GPS (1-PPS signal) and transmits data via GPRS/UDP to a remote server for visualization adhering to the **IEEE C37.118.1a-2014** standard.

**Features:**
* **Real-time Sync:** Uses GPS Pulse-Per-Second (PPS) to synchronize ADC sampling.
* **High Precision:** Achieves a Total Vector Error (TVE) of < 1%.
* **Full Stack:** Includes custom PCB design, embedded firmware, and a web-based monitoring dashboard.

---

##  System Architecture

The system is built around three core hardware modules:
1.  **Metrology:** Analog Devices **ADE7880** (Polyphase Multifunction Energy Metering IC).
2.  **Processing:** **STM32F103** (ARM Cortex-M3 Microcontroller).
3.  **Communication/Sync:** **SIM808** (GPS/GPRS Module).

### Block Diagram
The STM32 acts as the master, communicating with the ADE7880 via SPI for data acquisition and the SIM808 via UART for time synchronization and data transmission.

| Component | Function | Interface |
| :--- | :--- | :--- |
| **ADE7880** | RMS, Active/Reactive Power, Harmonics, Phase Angle calculation | SPI |
| **SIM808** | GPS Time Synchronization (PPS) & GPRS Data Transmission | UART |
| **STM32F103** | Data aggregation, Protocol formatting (IEEE C37.118), Control logic | SPI/UART |

---


