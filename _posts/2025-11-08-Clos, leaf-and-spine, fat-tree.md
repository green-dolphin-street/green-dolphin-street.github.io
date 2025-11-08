---
title: "Clos Topology, Leaf-and-Spine Architecture, and Fat-trees"
layout: single
date: 2025-11-08
categories:
  - IT Infrastructure Engineering
tags:
  - Network topology
  - Network
  - HPC
use_math: true
---

## Network Topology Deep Dive: From Crossbar to HPC

This document outlines the concepts of network topology, starting from the fundamental crossbar switch and evolving to the complex, large-scale networks used in modern High-Performance Computing (HPC).

### 1. The Original Context: Circuit Switching and Crossbar Switch
* **Circuit Switching**: method of creating a dedicated, persistent paths (a "circuit") for the entire duration of a communication (e.g., a phone call in a telephone network).
* The **Crossbar Switch** was the *device* used to physically build these circuits.
  * The simplest form of non-blocking switches
  * Connects a set of '$N$' inputs to '$M$' outputs, using a **grid of crosspoints**.
  * To connect any input to any available output, the switch at the corresponding crosspoint is activated.
* **Key Property:** It is "strictly non-blocking," meaning any input-to-output connection can be made without interfering with any other existing connection.
* **The Scaling Problem:** The number of required crosspoints (switches) scales quadratically ($N^2$, or $N \times M,$ if $N \neq M$).

### 2. More Scalable Design: The Clos Topology (invented by Charles Clos in 1953)

* **Clos network:** a multi-stage switching architecture that builds one large, non-blocking (or nearly non-blocking) switch network out of many smaller crossbar switches
* **Original Purpose: Circuit Switching**
    * Invented to solve the scaling problem for the circuit-switched telephone network.
    * Allowed telephone companies to connect entire cities using smaller, manageable crossbar switches
* **3 Stages:**
    1.  **Stage 1: Ingress** (Input switches)
    2.  **Stage 2: Middle** (Core switches)
    3.  **Stage 3: Egress** (Output switches)
* **Key Wiring Rule:** Every switch in one stage is connected to *every* switch in the next stage.
* **Path diversity:** multiple available paths for establishing a connection through the network. If one path through the Middle stage is busy, the switch can instantly use another, maintaining its non-blocking property at a fraction of the cost of a single giant crossbar.
* Transition to **Packet Switching**
  * For decades, Clos networks were not relevant for data networking.
  * The traditional 3-tier hierarchical model (Core, Distribution, Access) was mainly used. This was well-suited for the **north-south** (user-to-server) traffic.

### 3. The Modern Data Center Adaptation: Spine-and-Leaf
* **The New Problem:** The rise of computing clusters and data centers created a new traffic pattern: **East-West** (server-to-server) communication. 
* **The Solution:** The Clos topology was "rediscovered" as the perfect solution for this new **packet-switched** problem.
* **The Adaptation: A "Folded Clos Network"**
    * In a data center, a server is both an input (Ingress) and an output (Egress).
    * The 3-stage Clos is "folded" into a 2-tier hierarchy:
        * **Leaf Layer:** combination of the Ingress and Egress stages.
        * **Spine Layer:** the Middle stage.
* **Spine-and-Leaf connection rule:**
    1.  Servers connect to Leaf switches.
    2.  Every Leaf switch connects to *every* Spine switch.
    3.  Leaf switches *never* connect to other Leaf switches.
    4.  Spine switches *never* connect to other Spine switches.
* **Advantages:**
    * **Low, predictable latency:** All server-to-server (East-West) traffic is exactly 2 hops: **Leaf $\rightarrow$ Spine $\rightarrow$ Leaf**.
    * **Easy, Scalable Bandwidth:**
        * Need more server ports? $\Rightarrow$ Add a new Leaf switch and connect it to all the Spines.
        * Need more network bandwidth (capacity)? $\Rightarrow$ Add a new Spine switch and connect it to all the Leaves.

### 4. Physical vs Logical structure 

* A common confusion stems from the differences between the logical leaf-and-spine diagram and the physical build.
* **Logical structure:** Shows abstract layers of "Leaf" and "Spine" switches.
* **Physical structure (Top-of-Rack Design):**
    * **The Rack (Physical Unit):**
      * A single data center cabinet that contains 20-40 servers.
      * Contains 1 or 2 Leaf Switches **inside the same rack** (at the "Top-of-Rack," or ToR).
      * The "intra-rack" server interconnection (the leaf-to-spine connection) runs *inside* this one rack, from the servers to their own ToR/Leaf switch.
    * **The Fabric (Physical Interconnect):**
        * The **Spine Switches** live in their own separate, central racks.
        * Longer (often fiber optic) cables run from *every* Leaf switch (in *every* rack) to *every* Spine switch (in the central racks). These are the "fat branches."
    * **Conclusion:** The logical "Server" and "Leaf" layers are physically bundled together into a repeating unit (the rack).

### 5. HPC Nomenclature
#### 5.1. Fat-Tree: The HPC Term for Clos

* **A "Fat-Tree" *is* a Clos topology.** The terms are interchangeable in this context.
    * **Spine-and-Leaf:** The modern *data center* marketing term.
    * **Fat-Tree:** The *HPC / parallel computing* academic term.
* **Why "Fat-Tree"?**
    * A normal computing "tree" has a bottleneck at the root.
    * A "fat-tree" has links that get "fatter" (i.e., have more aggregate bandwidth) as you move up the hierarchy.
    * This "fatness" is achieved by the Clos wiring: the collection of *all* parallel links from the Leaf layer to the Spine layer creates one massive-bandwidth "fat trunk."

#### 5.2. Other HPC Topologies (Torus & Dragonfly)

* The TOP500 list also shows other topologies (Torus, Dragonfly) because they are used in custom-built "hero" supercomputers (e.g., *Frontier* uses HPE Slingshot, a Dragonfly topology).
* **Different Goal:** These are "direct-connect" networks, *not* switch-based like a fat-tree.
* **Trade-off:**
    * **Pro:** Extremely fast for "nearest-neighbor" communication (common in physics simulations).
    * **Con:** *Not* uniform latency. A node talking to a far-away node (across the torus) is much slower than talking to its direct neighbor, unlike the predictable 2-hop time in a fat-tree.

#### 5.3. Clarifying "k-ary n-tree" (Theory vs. Practice)

* "k-art n-tree" means a tree network with each node having '$k$' children and extending to depth '$n$'. (e.g., a 4-ary 3-tree).
* This is a *theoretical model* for analyzing fat-trees, which causes two main points of confusion in practice.

* **Confusion 1: "Consistent children nodes" (k-ary)**
    * **Theory:** Implies all switches are identical, with '$k$' ports (e.g., '$k/2$' up, '$k/2$' down).
    * **Practice:** This is often false. Hardware is *asymmetric*.
        * **Leaf Switches:** Have many *downlink* ports (e.g., 48x25G for servers) and fewer, high-speed *uplink* ports (e.g., 8x100G for spines).
        * **Spine Switches:** Have all high-speed ports that all connect "down" to Leaves (e.g., 64x100G ports).

* **Confusion 2: "Tree level can extend beyond 3" (n-tree)**
    * **This is very practical and common.** This is how you scale *beyond* the port limits of a single Spine switch.
    * **The Limit:** A 3-stage Clos is limited by the port density of its Spine switches. (A 64-port Spine switch can support a maximum of 64 Leaf switches).
    * **The Solution:** A **5-stage Clos** (or "Super-Spine" architecture).
        * You build multiple 3-stage "pods" (Leaf + Spine).
        * You add a new, third layer of switches (the **Super-Spine**) that connects *all* the Spine switches from *all* the pods.
    * **New Data Path (5 Hops):**
        Server A $\rightarrow$ Leaf (Pod 1) $\rightarrow$ Spine (Pod 1) $\rightarrow$ **Super-Spine** $\rightarrow$ Spine (Pod 2) $\rightarrow$ Leaf (Pod 2) $\rightarrow$ Server B 