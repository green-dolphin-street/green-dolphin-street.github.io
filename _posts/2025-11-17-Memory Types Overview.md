---
title: "Overview of Computer Memory Types"
layout: single
date: 2025-11-17
categories:
  - IT Infrastructure Engineering
tags:
  - Memory
use_math: true
---

### I. Volatile Memory (Data is lost when power is removed)
    * **RAM (Random Access Memory)**: Used for active data and immediate processing.
        * **SRAM (Static RAM)**
            * **Characteristics**: Fastest, most expensive, does not need refresh.
            * **Role**: Primary use in **CPU Cache** (L1, L2, L3) and Register Files.
        * **DRAM (Dynamic RAM)**
            * **Characteristics**: Slower than SRAM, high density, requires constant electrical refresh.
            * **Sub-Families**:
                * **DDR SDRAM** (e.g., DDR4, DDR5): Standard **Main System Memory** for desktops and servers.
                * **GDDR** (e.g., GDDR6): Optimized for **Graphics Processing** (GPUs); narrow bus, high speed.
                * **HBM (High Bandwidth Memory)**: **3D-Stacked** memory with an ultra-wide bus; high-performance compute and AI accelerators.

---

### II. Non-Volatile Memory (Data is retained when power is removed)
    * **ROM (Read-Only Memory)**: Used for permanent programs and firmware.
        * **PROM / EPROM / EEPROM**: Traditional types used for firmware and booting code.
    * **Flash Memory**: A high-density, electrically erasable type of ROM, forming the basis of modern storage.
        * **NOR Flash**: Faster read access, often used for **boot code and firmware** (BIOS/UEFI).
        * **NAND Flash**: Higher density, lower cost, used for **mass storage** (SSDs, USB drives, SD cards).
    * **Emerging NVRAM (Non-Volatile RAM)**: Aiming to combine the speed of RAM with the non-volatility of Flash.
        * **Examples**: **MRAM** (Magnetoresistive), **PRAM/PCM** (Phase-Change), **FeRAM** (Ferroelectric).
