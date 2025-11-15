---
title: "Physical implementation of Fat-tree, torus and Dragonfly topologies"
layout: single
date: 2025-11-15
categories:
  - IT Infrastructure Engineering
tags:
  - HPC
  - Topologies
  - Network
use_math: true
---

### Fat-Tree (Leaf-Spine)

* **Core Concept:** A hierarchical network topology, most commonly implemented as a **Leaf-Spine** architecture.
* **Physical Layout:**
    * **Leaf:** Servers in a rack connect (e.g., via DACs) to a Top-of-Rack (ToR) switch. This is the "Leaf" switch.
    * **Spine:** A set of core switches (the "Spine") sits at a higher tier, often in central or end-of-row racks.
* **Cabling Pattern:**
    * **Intra-Rack:** Servers connect to their local Leaf switch.
    * **Inter-Rack:** Every Leaf switch is physically cabled "up" to *every* Spine switch. These links are typically optical (AOCs or transceivers).
* **Key Implementation Detail:** Leaf switches **do not** connect to other Leaf switches. Spine switches **do not** connect to other Spine switches. All traffic between racks must go "up" to a Spine and "down" to the destination Leaf.

### 3D Torus

* **Core Concept:** A **logical grid**, not a physical one. Each node is assigned a logical coordinate `(x, y, z)` and is cabled to its six neighbors (`+x`, `-x`, `+y`, `-y`, `+z`, `-z`).
* **Physical Layout:** The logical dimensions are mapped onto the physical data center floor. There is no single standard, but a common mapping is:
    * **X-Dimension:** Connects nodes *within the same rack* (e.g., servers stacked vertically).
    * **Y-Dimension:** Connects nodes *between adjacent racks* in the same row.
    * **Z-Dimension:** Connects nodes *between adjacent rows* (e.g., crossing the hot/cold aisle).
* **Cabling Pattern:** This is a very dense and structured cabling plan. Each node must have six ports and cables.
* **Key Implementation Detail:** The "torus" (wrap-around) is implemented with very long physical cables that connect the "end" of a dimension back to the "beginning." For example, the last rack in a row (end of Y-dim) has a long optical cable running all the way back to the first rack in that row.

### Dragonfly

* **Core Concept:** A **group-based** topology that is *not* hierarchical. It's designed to reduce the number of long-distance, expensive cables.
* **Physical Layout:**
    * **Group:** The network is partitioned into "Groups." A group is typically one or two racks containing servers and a set of local switches.
    * **Intra-Group:** Within the group, the local switches are connected to each other, often in an **all-to-all mesh**, using short, cheap DACs.
    * **Inter-Group:** The switches use their remaining ports for "remote" links. These are long optical cables (AOCs) that connect *directly* to switches in *other* groups.
* **Cabling Pattern:** A dense mesh *inside* the group, and a sparser, structured set of connections *between* groups.
* **Key Implementation Detail:** There is no central "Spine" tier. A "Group" acts as a single, powerful virtual switch. Traffic hops from its source group directly to the destination group.

---

### Clarification: Fat-Tree vs. Dragonfly Implementation

You are correct that both use short "local" cables (DACs) and long "remote" cables (AOCs). The difference is *how* and *why*.

1.  **Hierarchy:**
    * **Fat-Tree:** Is **hierarchical**. The optical AOCs connect "up" from the Leaf (rack) to a shared, central Spine.
    * **Dragonfly:** Is **flat (group-to-group)**. The optical AOCs connect "across" from one group directly to another group. There is no central Spine.

2.  **Intra-Group Switch Connections (The Key Difference):**
    * **Fat-Tree:** The "group" (the rack) has **zero** switch-to-switch connections. The Leaf switch *only* connects to its local servers (down) and the Spines (up).
    * **Dragonfly:** The "group" is *defined* by its dense **intra-group switch-to-switch mesh**. The switches in a group are all connected to *each other* (e.g., all-to-all) using short DACs.
