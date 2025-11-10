---
title: "Storage Snapshots"
layout: single
date: 2025-11-10
categories:
  - IT Infrastructure Engineering
tags:
  - Storage
use_math: true
---

* **Core Concept:** An instantaneous, "point-in-time" logical copy of a volume. It's "instant" because it **does not copy the data** at the moment of creation.
* **How It Works (Common Methods):**
    * **Copy-on-Write (CoW):** When the snapshot is created, the original data is "frozen." When the system receives a request to *change* a block on the original volume (an overwrite), it first *copies* the *original* block to a separate snapshot storage area. Then, it allows the new write to "break the link" and overwrite the original block. The snapshot "view" is a combination of the "frozen" data and the old blocks saved in the snapshot area.
    * **Redirect-on-Write (RoW):** This is often more efficient. When a snapshot is taken, all *new* writes are simply redirected to new, empty blocks on the disk. The snapshot itself just maintains a "map" of pointers to the original blocks that existed at that exact moment. The original blocks are never overwritten.
* **Key Characteristics:**
    * **Space-Efficient:** A snapshot initially consumes almost no space. It only grows as data on the *original* volume is changed, consuming space to store the original (CoW) or new (RoW) blocks.
    * **Not a Backup:** A snapshot is not a backup. It is a set of pointers and data on the *same* physical system. If the storage array fails, both the original volume and all its snapshots are lost.
    * **Use Cases:** Used for quick "rollbacks" before a risky software patch, and to provide a stable, "frozen" point-in-time for backup software to read from without interrupting the live application.
