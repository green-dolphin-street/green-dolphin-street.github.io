---
title: "DIMM and its Types (UDIMM, RDIMM, and LRDIMM)"
layout: single
date: 2025-11-11
categories:
  - IT Infrastructure Engineering
tags:
  - Availability
use_math: true
---

### DIMM?
* **DIMM** stands for **Dual In-line Memory Module**.
* It is the physical circuit board—the "stick"—that holds RAM (Random Access Memory) chips.
* This is the standard form factor used in desktop PCs, workstations, and servers.

### Different Shapes (Form Factors) & Keying
* **DIMM:** The full-size, long module used in desktops and servers.
* **SO-DIMM (Small Outline DIMM):** A much shorter, more compact module (about half the length of a DIMM) used in laptops, mini-PCs, and some network devices.
* **Keying (Notches):** The position of the notch on the pin-contact edge is different for each memory generation (e.g., **DDDR3**, **DDR4**, **DDR5**). This physically prevents you from installing an incompatible generation of RAM into a motherboard slot.

### DIMM Types (UDIMM vs. RDIMM vs. LRDIMM)
* **UDIMM (Unbuffered DIMM):**
    * This is the standard, "unbuffered" memory for **consumer desktops** and workstations.
    * All signals travel directly from the CPU's memory controller to the memory chips.
    * This direct path has the lowest latency but puts a high electrical load on the CPU, limiting the total amount of RAM a system can stably support.
* **RDIMM (Registered DIMM):**
    * This is the standard for **mainstream servers**.
    * It has a **Registered Clock Driver (RCD) chip** on the module.
    * This chip buffers the **address and control signals** (but *not* the data).
    * This greatly reduces the load on the memory controller, allowing the system to stably support many more RAM modules (i.e., higher total system capacity).
* **LRDIMM (Load-Reduced DIMM):**
    * This is used in **high-capacity servers** (e.g., large databases, HPC).
    * It has a **Memory Buffer (MB) chip** on the module.
    * This chip buffers **all signals** (address, control, *and* data).
    * This offers the absolute lowest electrical load, enabling the system to support the maximum possible memory density and total capacity.

* **MRDIMM (Multiplexed Rank DIMM)**
    * A cutting-edge memory technology designed specifically for **AI and HPC** (High-Performance Computing).
    * Unlike RDIMMs which access one rank (group of chips) at a time, MRDIMM uses a special data buffer (Multiplexer) to access **two ranks simultaneously**.
    * It combines these two data streams into a single, super-fast burst. This effectively **doubles the bandwidth (speed)** compared to standard DDR5 RDIMMs without needing faster individual memory chips.
    * Currently, up to two ranks can be accessed simultaneously, but future advancements may allow for even more.
    * **Physical Difference:** Often uses a **Tall Form Factor (TFF)**—it is physically taller than a standard DIMM to accommodate the extra buffer chips and heat management components.