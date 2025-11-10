---
title: "De-duplication"
layout: single
date: 2025-11-10
categories:
  - IT Infrastructure Engineering
tags:
  - Storage
use_math: true
---

* **Core Concept:** A data reduction technique that finds and eliminates redundant data blocks, storing only one unique copy.
* **How It Works:**
    1.  The storage system breaks incoming data into "chunks" or "blocks" (either fixed-size or variable-size).
    2.  It runs a **hash algorithm** (like SHA-1) on each block to create a unique digital fingerprint.
    3.  It looks up this hash in an index table.
    4.  **If the hash is new:** The block is unique. It's written to disk, and its hash is added to the index.
    5.  **If the hash already exists:** The block is a duplicate. The system discards the new block and simply creates a small *pointer* to the block that's already stored.
* **Implementation Types:**
    * **Inline:** De-duplication happens *as* data is being written. This saves space immediately but can add a small performance overhead to write operations.
    * **Post-process:** Data is written to disk first, and a background process runs later (e.g., at night) to scan for and remove duplicates. This has no impact on write performance but means storage is used inefficiently for a short time.
* **Best Use Cases:** Extremely effective for VDI (Virtual Desktops) where thousands of users have identical OS files, and for backup repositories that store many full-backups of similar data.
