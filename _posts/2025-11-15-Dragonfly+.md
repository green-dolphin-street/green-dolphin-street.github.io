---
title: "Dragonfly+"
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

* Dragonfly+ is a hybrid, hierarchical network topology designed for exascale-class supercomputers.
* It is a "hybrid" because it combines two different topologies at two different levels:
    * **Global Level:** It adopts the **Dragonfly** topology in connecting the large **groups** of nodes. Every group has a high-bandwidth optical remote links to every other group.
    * **Local Level (Intra-Group):** It uses a **Fat Tree** (also known as a Leaf-Spine or Clos network) to connect all the nodes *within* a single group.

* **How it Differs from Classic Dragonfly:**
    * The single, most important difference is the **internal topology of the local group**.
    * **Classic Dragonfly** (used in systems like HPE's Slingshot): Connects all routers *within* a group in a **full-mesh** (all-to-all).
    * **Dragonfly+:** Replaces that local full-mesh with a **Fat Tree** topology.

* **Who Proposed It?**
    * The Dragonfly+ architecture is primarily proposed, championed, and implemented by **NVIDIA** for its large-scale InfiniBand networking platforms. It is a key part of their strategy for building exascale systems using NDR (Next Data Rate) InfiniBand.

* **HPC Systems Using It:**
    * It is designed for the largest NVIDIA InfiniBand deployments.
    * The most prominent example is the **JUPITER Booster** system, the exascale supercomputer for EuroHPC located at the Jülich Supercomputing Centre (JSC).

* **Advantages of Dragonfly+:**
    * **Massive Scalability:** A Fat Tree can support far more nodes *within* a single group than a full-mesh (which is limited by switch port count, or "radix"). This allows the total system to scale to hundreds of thousands of nodes, far beyond a classic Dragonfly using the same switches.
    * **Cost-Effectiveness:** It uses standard, mass-market Fat Tree (Leaf-Spine) components and design principles for the local groups. This is more cost-effective and easier to build than the complex cabling of a very large full-mesh.
    * **High Performance:** It maintains the primary benefit of the Dragonfly (low diameter, high global bandwidth for inter-group traffic) while providing the well-understood, high, non-blocking bandwidth of a Fat Tree for all local (intra-group) traffic.