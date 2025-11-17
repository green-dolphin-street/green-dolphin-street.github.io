---
title: "Thermal Design Power (TDP) of CPUs"
layout: single
date: 2025-11-17
categories:
  - IT Infrastructure Engineering
tags:
  - CPU
  - Power Management
use_math: true
---


### Definition of TDP
* TDP stands for **Thermal Design Power**.
* **Core Function**: Represents the maximum amount of heat (measured in Watts) that a computer chip (CPU/GPU) is expected to generate under a heavy, sustained workload.

### Physics: Heat vs. Power
* **Unit of Measure**: Measured in **Watts** (W).
* **Conversion**: While it measures heat, it correlates closely with power consumption (1 Watt of electricity consumed $\approx$ 1 Watt of heat generated).
* **Crucial Distinction**: TDP is the *nominal* requirement for cooling, not the absolute maximum electrical power the chip will ever draw.

### Modern Nuances (Turbo & Boost)
* **Base Frequency**: TDP is typically calculated based on the chip running at its standard "Base" speed.
* **Boost Behavior**: Modern CPUs automatically overclock themselves (Turbo/Boost) when thermal headroom is available.
    * **Intel**: Distinguishes between **PL1** (Long duration, usually equals TDP) and **PL2** (Short duration, can be 2x TDP).
    * **AMD**: Uses **PPT** (Package Power Tracking), where the actual power limit is often ~30% higher than the box TDP (e.g., 105W TDP $\rightarrow$ 142W max draw).

### Practical Applications for Builders
* **Selecting a Cooler**:
    * The cooler's thermal rating must **exceed** the CPU's TDP.
    * *Example*: For a 125W TDP CPU, use a cooler rated for 150W+ to ensure quiet and cool operation.
* **Selecting a PSU**:
    * Do not use TDP as the exact number for power consumption.
    * *Guideline*: Assume the peak power draw will be higher than the TDP (often 1.25x to 1.5x) when calculating total system wattage.
