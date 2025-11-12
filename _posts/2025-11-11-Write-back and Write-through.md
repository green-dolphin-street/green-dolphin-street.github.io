---
title: "Write-back and Write-through in RAID"
layout: single
date: 2025-11-11
categories:
  - IT Infrastructure Engineering
tags:
  - Storage
  - RAID
  - Availability
use_math: true
---

###  Write-Through Mode (Prioritizes Safety)

* **How it works:** When data is sent, the RAID controller writes it to its cache *and* directly to the physical disks (HDDs or SSDs) at the same time.
* **Acknowledgement:** The controller only sends the "write complete" signal back to the operating system *after* it gets confirmation that the data is safely on the physical disks.
* **Pros:**
    * **High Data Safety:** If the server loses power, the data is already on the disks. There is no risk of data loss from a power failure.
* **Cons:**
    * **Slower Performance:** The system must wait for the write to complete on the slowest part of the chain (the physical disks).

---

### Write-Back Mode (Prioritizes Performance)

* **How it works:** When data is sent, the controller writes it *only* to its fast onboard cache.
* **Acknowledgement:** The controller *immediately* sends the "write complete" signal back to the operating system, as if the write were finished.
* **Background Action:** The controller "flushes" (writes) the data from its cache to the physical disks later, when it's convenient.
* **Pros:**
    * **Extremely Fast Performance:** The operating system and applications experience near-instant write speeds.
* **Cons:**
    * **High Risk of Data Loss:** If power is lost *after* the data is in the cache but *before* it's flushed to the disks, that data is permanently lost.
    * **Mitigation:** This mode should **only** be used if the controller has a Battery Backup Unit (BBU) or supercapacitor to keep the cache powered during an outage.

