---
title: "Network Connector Interfaces"
layout: single
date: 2025-11-08
categories:
  - IT Infrastructure Engineering
tags:
  - Network
use_math: true
---

This document explains the two main types of cables DAC and AOC, and how they plug into network ports via connector interface such as OSFP and QSFP(-DD).

### 1. DAC (Direct Attach Copper) Cables

* **What It Is:** A **DAC** is an all-electrical cable. It's essentially a high-speed, shielded **copper** wire (called twinax) with the transceiver connectors (like QSFP-DD) permanently attached to both ends.
* **Typical Use (Intra-Rack):** DACs are for short-distance connections, typically from **1 to 7 meters**. Their primary use is connecting servers to the Top-of-Rack (ToR) switch *within the same server rack*.
* **Two Types:**
    * **Passive DAC (1-3m):** The most common type. It's just a "dumb" wire. It's cheap, has the lowest latency, and consumes **zero power** because there are no electronics.
    * **Active Electrical Cable (AEC) (3-7m):** This is a type of "active" DAC. It has small chips inside the connector heads to boost and clean the electrical signal, allowing it to travel reliably over a few more meters of copper. It consumes a very small amount of power.

### 2. AOC (Active Optical Cable) Cables

* **What It Is:** An **AOC** is an **optical fiber cable** with the transceiver connectors (like QSFP-DD) permanently attached to both ends.
<!-- * **How It Works:** Inside the connector head is a tiny, built-in transceiver that converts the switch's electrical signal to an optical (light) signal. The light travels down the fiber, and the connector on the other end converts it back to an electrical signal for the other device. -->
* **Typical Use (Inter-Rack):** AOCs are used for medium-distance connections, typically from **10 to 100 meters**. They are perfect for connecting switches in different racks or rows within the same data center room.

### 3. Standardized Network Interface Form Factors (OSFP & QSFP(-DD))

* **A Versatile Socket:** The OSFP or QSFP(-DD) **port on the switch** is a standardized **electrical socket**. It provides:
    1.  **Power** (to run the chips in an AOC or AEC).
    2.  **An Electrical Data Path** (the 8 high-speed lanes).
    3.  **Conversion Between Optical/Electrical Signals** (depends on what you plug in).

* **One Port, Many Options:** This design gives you maximum flexibility. You can look at any empty OSFP or QSFP-DD port and decide what you need to plug in:
    * **Need to connect a server 2 meters away?** Plug in a cheap, passive **DAC**.
      * In this case, the cable 'head' is merely in the shape of BGIC/SFPs, but it's just a copper wire inside, lacking the electronic parts that convert optical/electrical signals.
    * **Need to connect to the next rack 7 meters away?** Plug in an **AEC**.
    * **Need to connect to a switch 50 meters away?** Plug in an **AOC**.
    * **Need to connect to another building 2km away?** Plug in a **pluggable optical transceiver** (like a 400G-LR4) and then attach a separate fiber cable to it.

The interface is the common "outlet" that allows all these different cable types to connect to the network device

#### 3.1. Key High-Level Differences Between the QSFP-DD and OSFP form factors

Both OSFP and QSFP-DD are modern form factors designed for high-speed 400G and 800G networking. Both use an 8-lane electrical interface to achieve these speeds (e.g., $8 \times 50\text{G}$ or $8 \times 100\text{G}$).

Their main differences are based on a trade-off between size, power, and compatibility.

* **Physical Size:**
    * **OSFP (Octal Small Form-factor Pluggable):** Is a new, larger form factor. It is slightly wider, longer, and thicker than a QSFP module.
    * **QSFP-DD (Quad Small Form-factor Pluggable Double Density):** Is smaller. It intentionally maintains the *exact same* physical dimensions as the older, widely-adopted 4-lane QSFP (like QSFP28) modules.

* **Thermal Management & Power:**
    * **OSFP:** Its larger size allows for an **integrated heat sink** directly on the module. This provides superior thermal management, enabling it to support higher-power modules (typically 15W and higher).
    * **QSFP-DD:** Its compact size limits it to lower-power modules (typically in the 7-12W range) and it relies more on the switch's system-level cooling.

* **Backward Compatibility:**
    * **QSFP-DD:** This is its primary advantage. A QSFP-DD port is fully backward compatible with older 4-lane QSFP+ (40G) and QSFP28 (100G) modules.
    * **OSFP:** As a new form factor, its ports are **not** backward compatible with any previous QSFP modules.

* **Port Density:**
    * **QSFP-DD:** The smaller module size allows for higher port density on a 1U switch faceplate (typically 36 ports).
    * **OSFP:** The larger size results in a slightly lower port density (typically 32 ports).

#### 3.2. How "Double Density" Works: The Pinout

The "double density" is achieved with a clever design on the module's electrical edge connector (the part that plugs into the switch).

* **Standard QSFP (4-Lane):**
    * Has a **single row** of gold-plated electrical contacts on its edge.
    * This one row provides the 4 electrical lanes.

* **QSFP-DD (8-Lane):**
    * It has the *exact same* front row of contacts as the standard QSFP.
    * It adds a **second, recessed row** of contacts just behind the first one.
    * This second row provides the 4 *additional* lanes, doubling the total to 8.

* **This is the key to backward compatibility:**
    * When you plug in an old 4-lane QSFP28 module, it only has the front row of contacts. The port engages with these and works at 100G.
    * When you plug in a new 8-lane QSFP-DD module, the port is designed to engage with *both* rows of contacts, see all 8 lanes, and run at 400G or 800G.

#### 3.3. Brief Evolution History of Pluggable Form Factors
The primary drivers for this evolution have always been:
1.  **Increased Speed:** From 1 Gbps to 400G/800G and beyond.
2.  **Increased Port Density:** Fitting more ports onto the front of a 1U switch.

##### 3.3.1. The Origin: GBIC (1 Gbps)

* **Name:** GBIC (Giga-bit Interface Converter)
* **Speed:** 1 Gbps
* **Role:** The "grandfather" of all modern transceivers. It was one of the first popular, hot-swappable modules that let network engineers easily connect switches with different cable types (e.g., short-range copper or long-range fiber).
* **Legacy:** It was large, and its size limited the number of ports you could fit on a switch. It is now completely obsolete.

##### 3.3.2. The "Mini" Revolution: SFP (1 Gbps)

* **Name:** SFP (Small Form-factor Pluggable)
* **Speed:** 1 Gbps
* **Role:** The direct replacement for the GBIC. It provided the same 1 Gbps speed but in a *much* smaller form factor.
* **Key Feature:** Because it was so much smaller, it dramatically increased port density, allowing for 24, 48, or more ports on a single switch.
* **Nickname:** Due to its role, it is still commonly called a **"Mini-GBIC"**.

##### 3.3.3. The 10G/25G Upgrade: SFP+ and SFP28

This family kept the *exact same physical size* as the SFP, but upgraded the electronics for higher speeds.

* **SFP+ (Enhanced SFP):**
    * **Speed:** 10 Gbps
    * **Role:** The dominant form factor for 10G connections, especially for server-to-switch links.

* **SFP28:**
    * **Speed:** 25 Gbps
    * **Role:** An upgrade path from 10G, offering 2.5x the speed in the same form factor. It became a very efficient building block for 100G.

##### 3.3.4. Going Multi-Lane: QSFP (40G & 100G)

As speeds increased, a new, slightly larger form factor was needed to bundle multiple lanes together.

* **Name:** QSFP (Quad Small Form-factor Pluggable)
* **"Quad" Means 4:** This form factor bundles 4 data lanes in one module.

* **QSFP+:**
    * **Speed:** 40 Gbps ($4 \text{ lanes} \times 10\text{G}$)
    * **Role:** The standard for 40G. Also popular for "breakout" cables, where one 40G port could split into four 10G SFP+ ports.

* **QSFP28:**
    * **Speed:** 100 Gbps ($4 \text{ lanes} \times 25\text{G}$)
    * **Role:** The workhorse of the modern data center. It made 100G dense, affordable, and the standard for switch-to-switch connections.



##### 3.3.5. The 8-Lane Era: QSFP-DD and OSFP (400G & 800G)

To get to 400G and beyond, the industry needed 8-lane modules. This led to two competing standards.

* **QSFP-DD (Quad... Double Density):**
    * **Speed:** 400G ($8 \times 50\text{G}$) and 800G ($8 \times 100\text{G}$)
    * **Role:** The "evolution" approach. It keeps the **same physical size as the QSFP28** but adds a second row of electrical contacts (the "Double Density") to get 8 lanes.
    * **Key Feature:** Backward compatible with older QSFP28 modules.

* **OSFP (Octal Small Form-factor Pluggable):**
    * **Speed:** 400G ($8 \times 50\text{G}$) and 800G ($8 \times 100\text{G}$)
    * **Role:** The "revolutionary" approach. "Octal" means 8 lanes. It's a new, slightly larger form factor designed from the ground up for the high power and thermal needs of 8-lane optics.
    * **Key Feature:** Not backward compatible, but its larger size gives it better thermal performance for higher-power modules.

##### 3.3.6 Size Analogy
- GBIC: The largest 'brick' cell phones from the 1990s
- SFP: The early 2000s 'candy bar' sized phones
- QSFP, OSFP: The slightly larger sized modern phones. Saying that OSFP is larger than QSFP is like saying an iPhone Pro Max is larger than iPhone SE.

### References
- [QSFP-DD vs OSFP](https://www.heyoptics.net/blogs/wiki/qsfp-dd-vs-osfp-what-are-the-differences)