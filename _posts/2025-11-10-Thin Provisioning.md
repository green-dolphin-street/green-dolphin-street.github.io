---
title: "Thin Provisioning"
layout: single
date: 2025-11-10
categories:
  - IT Infrastructure Engineering
tags:
  - Storage
use_math: true
---

* **Core Concept:** A method of *allocating* storage on-demand. It allows you to *present* a logical volume (LUN) to a server that is much larger than the *actual* physical disk space consumed.
* **How It Works:**
    1.  You create a 10TB volume for a server.
    2.  The storage system tells the server's OS, "You have 10TB of space."
    3.  Physically, the system reserves almost no space (perhaps just a few megabytes for metadata).
    4.  As the server writes data, the storage system allocates physical blocks from its main pool "just-in-time" to store that data.
    5.  If the server has only written 50GB of data, the volume *looks* like 10TB to the server but *consumes* only 50GB of physical pool space.
* **Key Consideration (Over-subscription):** This technique's main benefit is also its main risk. You can provision *more* total storage (e.g., 50TB of thin volumes) than you physically own (e.g., a 20TB physical pool). This works as long as not everyone tries to use their full allocation.
* **Management:** This requires active monitoring. Administrators must track the *subscription rate* and *actual consumption* to ensure the physical pool doesn't run out of space, which would cause all volumes trying to write new data to fail.
