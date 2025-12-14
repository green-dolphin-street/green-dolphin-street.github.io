---
title: "PCIe vs. SXM Form Factors"
layout: single
date: 2025-12-05
categories:
  - IT Infrastructure Engineering
tags:
  - GPU
  - AI
use_math: true
---

### PCIe vs. SXM Form Factors

* **PCIe (Peripheral Component Interconnect Express)**
    * **Physical:** Standard expansion card that slots perpendicularly into a motherboard (like a gaming GPU).
    * **Connectivity:** Communicates via PCIe lanes; bandwidth is limited by the PCIe bus generation (e.g., Gen5).
    * **Power & Thermals:** typically capped around 350W–400W; relies on server airflow.
    * **Use Case:** Inference, mainstream enterprise servers, VDI, lighter training.
* **SXM (Server Express Module)**
    * **Physical:** Mezzanine socket form factor that lies flat on a specialized baseboard.
    * **Connectivity:** Communicates via NVLink/NVSwitch directly on the board; offers massive bandwidth (up to 900GB/s).
    * **Power & Thermals:** Supports very high power draw (700W+ per GPU); often liquid-cooled or requires high-performance air cooling.
    * **Use Case:** Large-scale LLM training, supercomputing, foundation models.

### SXM and HBM (High Bandwidth Memory)

* **Relationship:** They are distinct technologies but highly correlated.
    * **SXM:** The physical socket/power delivery method.
    * **HBM:** The memory architecture on the GPU die.
* **Why they go together:** SXM provides the necessary power and thermal management to run HBM at maximum speed and capacity (e.g., HBM3e).
* **PCIe Exception:** Some PCIe cards (like H100 PCIe) *do* use HBM, but often at lower specs or capacities than their SXM counterparts due to power constraints.

### DGX vs. HGX Platforms

* **HGX (The Baseboard)**
    * **Definition:** A reference baseboard design containing 4 or 8 SXM GPUs + NVSwitches.
    * **Business Model:** Sold to OEM partners (Dell, HPE, Supermicro, Lenovo).
    * **Implementation:** Partners build their own chassis, cooling, and power solutions around this board.
* **DGX (The Appliance)**
    * **Definition:** A complete, turnkey server system manufactured by Nvidia.
    * **Composition:** Contains an HGX board inside, plus Nvidia's proprietary chassis, cooling, and pre-installed software stack.
    * **Business Model:** Sold directly by Nvidia as a premium, "plug-and-play" AI supercomputer building block.

### Scale Up vs. Scale Out

* **Scale Up (Intra-node)**
    * **Definition:** Connecting multiple GPUs (usually 8) within a single machine (node) to act as one logical unit.
    * **Technology:** Uses NVLink and NVSwitch (inside the DGX/HGX).
    * **Goal:** Maximizing memory pool and compute for a single large model layer.
* **Scale Out (Inter-node)**
    * **Definition:** Connecting many separate server nodes together to form a cluster.
    * **Technology:** Uses InfiniBand or Ethernet (Spectrum-X) networking cables between servers.
    * **Goal:** Parallelizing training across hundreds or thousands of machines.

### Nvidia Series Availability

* **H-Series (Hopper)**
    * **SXM:** Yes (Standard for training, e.g., H100 SXM).
    * **PCIe:** Yes (For inference/standard servers, e.g., H100 NVL).
* **B-Series (Blackwell)**
    * **Module/SXM:** Yes (Focus is on the GB200 Superchip module and HGX B100/B200).
    * **PCIe:** Yes (Planned B200 PCIe versions for standard integration).
* **L-Series (Lovelace)**
    * **SXM:** No (Generally not available in SXM/HGX formats).
    * **PCIe:** Yes (Exclusively PCIe, e.g., L40S).
    * **Focus:** Omniverse, Digital Twins, Graphics, and Entry/Mid-range Inference.
