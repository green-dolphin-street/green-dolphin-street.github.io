---
title: "Physical Tear of SSDs and Metrics for Their Endurance"
layout: single
date: 2025-12-05
categories:
  - IT Infrastructure Engineering
tags:
  - SSD
  - Storage
use_math: true
---

### Endurance Metrics: TBW & DWPD

* **TBW (Terabytes Written)**
    * **Definition:** The cumulative amount of data that can be written to the SSD over its warrantied lifespan before the error rate exceeds the ECC (Error Correction Code) capability.
    * **Role:** Acts as the "odometer" of the drive.
    * **Context:** Primarily used for consumer drives or warranty validation.

* **DWPD (Drive Writes Per Day)**
    * **Definition:** A normalized metric indicating how many times you can overwrite the entire capacity of the drive daily for the duration of the warranty (typically 5 years).
    * **Formula:**
        $$DWPD = \frac{TBW}{Capacity \times WarrantyDays}$$
    * **Context:** Crucial for Enterprise/Data Center comparisons.
        * *Read-Intensive drives:* ~0.3 to 1 DWPD.
        * *Mixed-Use drives:* ~3 DWPD.
        * *Write-Intensive drives:* ~10+ DWPD.

---

### Why Only SSDs? (Magnetic vs. Electronic)

* **HDD (Magnetic Recording)**
    * **Physics:** Relies on flipping magnetic domains (dipoles) on a ferromagnetic platter.
    * **Wear Mechanism:** Mechanical (bearings, actuator arms) rather than media exhaustion. The hysteresis loop of the magnetic material does not degrade significantly with magnetic reversals.
    * **Result:** TBW is rarely cited because the mechanics usually fail before the platter loses the ability to hold a magnetic charge.

* **SSD (Charge Storage)**
    * **Physics:** Relies on trapping electrons in a quantum potential well.
    * **Wear Mechanism:** Destructive High-Field stress on the dielectric insulator.
    * **Result:** Every write event causes atomic-level damage, making endurance a finite, calculable resource.