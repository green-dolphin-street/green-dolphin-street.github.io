---
title: "Switched Fabric Networks"
layout: single
date: 2025-11-15
categories:
  - IT Infrastructure Engineering
tags:
  - Network Topology
  - Network
use_math: true
---


### Switched Fabric Networks

* **Definition:** A network architecture where end-nodes (like servers or storage) do not connect directly to each other. Instead, they all connect to dedicated, external network switch devices.
* **Core Concept:** The "fabric" is the collection of switches and their inter-switch links. The switches are responsible for receiving, processing, and forwarding data packets to their intended destination.
* **Key Characteristics:**
    * **Centralized Intelligence:** Switches manage the data paths.
    * **Scalability:** Can be scaled to large sizes by adding more switches in a structured way.
    * **High Performance:** Allows for many simultaneous, non-blocking conversations between nodes.
* **InfiniBand as a Switched Fabric:**
    * InfiniBand is a *standard* for building a high-performance switched fabric.
    * It relies on a central "brain" called the **Subnet Manager (SM)**.
    * The SM discovers all switches and nodes in the fabric and then calculates and programs the forwarding tables in every switch to implement a specific, efficient routing topology.
* **Common Switched Topologies:**
    * **Fat-Tree (or Clos Network):** The most common topology for InfiniBand in HPC/AI. It's a hierarchical design that provides high bisectional bandwidth (all nodes can communicate with all other nodes at high speed).
    * **Dragonfly:** A popular alternative to fat-tree for very large-scale systems. It groups switches into "pods" and uses high-speed links to connect the pods, which can reduce cost and cabling.

### Non-Switched Fabric (Switchless) Networks

* **Definition:** A network architecture where nodes are connected *directly* to each other without relying on external, standalone switches.
* **Core Concept:** The routing intelligence and switching logic are built *directly into* the network adapter (NIC) of each node. These NICs have multiple ports to connect to their neighbors.
* **Key Characteristics:**
    * **Distributed Intelligence:** Each node is responsible for routing and forwarding data.
    * **Potentially Lower Latency:** A packet can go directly from node to node without an external switch hop.
    * **Cabling Complexity:** Can become very complex to cable as the node count grows.
* **Common Non-Switched Topologies:**
    * **Bus:** An old topology where all nodes share a single, common communication line. Prone to collisions.
    * **Daisy Chain:** Nodes are connected in a simple line, one after the other.
        * This has a distinct start and end.
        * A single break in the chain (e.g., one node failing) severs the network for all nodes "downstream."
    * **Ring:** A special type of daisy chain where the **last node connects back to the first node**, forming a closed loop.
        * This lack of an "end" provides basic fault tolerance. If one link breaks, data can often travel the other way around the ring to reach its destination.

### The Torus: A Special Case

The "torus" topology is ambiguous and can be implemented in multiple ways, which is why it's often a point of confusion.

* **Switchless (Direct) Torus:** This is the *classic* definition. Each node has a multi-port NIC and is cabled directly to its neighbors in a 2D or 3D grid, with the edges "wrapping around." This is a **non-switched** design.
* **Switched Torus:** You can also build a torus using *switches*. In this design, the *switches* are connected to each other in a torus pattern, and the compute nodes connect to their local switch. This is a **switched fabric** design.
* **Logical Torus (via InfiniBand):** This is the most flexible. You can build a physical **fat-tree** (a switched fabric), and then program the **Subnet Manager (SM)** to create routing paths that *simulate* a torus. For applications optimized for a torus, this *logical* torus can be very efficient, even though the *physical* wiring is a fat-tree.