---
title: "Rpeak vs Rmax in HPC Performance"
layout: single
date: 2025-11-17
categories:
  - IT Infrastructure Engineering
tags:
  - HPC
  - Performance Metrics
use_math: true
---

### 1. Rpeak (Theoretical Peak Performance)
* **Definition**: The theoretical maximum speed a supercomputer can achieve if every single component operates at 100% capacity without any bottlenecks.
* **How it is Derived**: It is **calculated** mathematically based on hardware specifications, not measured by running software.
    * **Formula**: `(Frequency) × (Total Cores) × (FLOPs per Cycle)`
* **Significance**: It represents the absolute "hardware ceiling" or potential of the system.

### 2. Rmax (Maximal LINPACK Performance)
* **Definition**: The actual, sustained performance of the system when solving a real-world mathematical problem.
* **How it is Derived**: It is **measured** by running the **HPL (High Performance Linpack)** benchmark.
    * The benchmark solves a dense system of linear equations using **FP64 (Double Precision)**.
* **Significance**: This is the official score used to determine the ranking order on the **TOP500** list.

### 3. Key Differences
* **Reality vs. Theory**:
    * **Rpeak** is what the brochure says the hardware *could* do.
    * **Rmax** is what the system *actually* does under load.
* **Efficiency**:
    * The difference between the two reveals the system's efficiency.
    * **Efficiency Ratio**: `(Rmax / Rpeak) × 100`
    * A larger gap between Rpeak and Rmax usually indicates bottlenecks in the network interconnect, memory bandwidth, or thermal throttling.
