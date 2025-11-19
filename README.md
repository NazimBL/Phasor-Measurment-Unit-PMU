# Low-Cost M-Class Phasor Measurement Unit (PMU)

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Standard](https://img.shields.io/badge/IEEE-C37.118-green.svg)
![Hardware](https://img.shields.io/badge/Hardware-STM32%20%7C%20ADE7880-orange)

##  Project Overview
This repository contains the complete hardware design, firmware, and software implementation of a **Low-Cost M-Class Phasor Measurement Unit (PMU)**.

Designed for smart grid research and educational purposes, this device measures voltage and current phasors, frequency, and power metrics in real-time. It synchronizes measurements using GPS (1-PPS signal) and transmits data via GPRS/UDP to a remote server for visualization, strictly adhering to the **IEEE C37.118.1a-2014** standard.

**Features:**
* **Real-time Sync:** Uses GPS Pulse-Per-Second (PPS) to synchronize ADC sampling.
* **High Precision:** Achieves a Total Vector Error (TVE) of < 1%.
* **Full Stack:** Includes custom PCB design, embedded firmware, and a web-based monitoring dashboard.

---

## system Architecture

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

## Firmware & Custom Libraries

The firmware is developed using **MikroC PRO for ARM**. To handle the complexity of the metrology and communication modules, I developed modular drivers that abstract the low-level register operations.

###  ADE7880 Library (Metrology)
Interfacing with the **Analog Devices ADE7880** is challenging because its DSP registers vary in length (8, 16, 24, and 32 bits). I wrote a custom library (`ADE7880.c`) to handle these SPI transactions automatically.

**Key Features:**
* **Multi-bitwidth SPI Wrapper:** Automatically handles 8/16/24/32-bit read/write operations to DSP registers.
* **DSP Initialization:** Configures the `Run` register and DSP offsets.
* **Measurement Abstraction:** Simple functions to get human-readable values (RMS, Power, Phase Angle).

**Usage Example:**
The `getVRMS` function abstracts the SPI complexity to read the Root Mean Square voltage for a specific phase (A, B, or C):

```c
// From library/ADE7880.c
unsigned long getVRMS(char phase) {
    if (phase == 0) return ADE_Read32(AVRMS); // Reads 32-bit register for Phase A
    else if (phase == 1) return ADE_Read32(BVRMS);
    else return ADE_Read32(CVRMS);
}
