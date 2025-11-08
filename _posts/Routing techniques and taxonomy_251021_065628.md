### Routing Methods: IGP and EGP (English Summary)

#### 1. Routing Overview

* **Definition:** The process of determining the **optimal path** and forwarding packets from source to destination across a network (Layer 3 function).
* **Router's Role:** Uses the Routing Table to decide the Next Hop for a packet.

#### 2. Classification Basis

* Routing protocols are primarily classified based on the **AS (Autonomous System)** where the routers operate.
* **AS (Autonomous System):** A collection of routers under a single technical administration and policy (e.g., one ISP, an entire corporate network). Identified by a unique AS Number.

#### 3. IGP (Interior Gateway Protocol)

* **Role:** Used **only within a single AS**, aiming to find the best paths to all destinations inside that AS.
* **Characteristics:**
    * **Scale:** Suitable for relatively **smaller, uniform** networks within an AS.
    * **Goal:** Focuses on finding the **best path** based on technical metrics (e.g., speed, bandwidth, reliability).
    * **Decision Making:** Calculates a **Metric (cost)** for each path to select the optimal one.
* **Key Protocols:**
    * **OSPF (Open Shortest Path First):** Link-State protocol. Shares a complete map of the network to calculate the shortest path.
    * **EIGRP (Enhanced Interior Gateway Routing Protocol):** Hybrid protocol (Cisco proprietary, now open). Faster convergence than OSPF, often simpler to configure.
    * **RIP (Routing Information Protocol):** Distance Vector protocol. Simplest and oldest, based on hop count.

#### 4. EGP (Exterior Gateway Protocol)

* **Role:** Used to exchange routing information **between different ASes**, establishing communication paths across AS boundaries.
* **Characteristics:**
    * **Scale:** Essential for **large-scale** networks, forming the backbone of the Internet.
    * **Goal:** Focuses on **Policy** and **Boundary Control**, rather than technical optimality. (e.g., deciding which ISP to send traffic through based on business rules).
    * **Decision Making:** Path selection heavily relies on the **AS-Path (sequence of ASes traversed)** and administrative policies.
* **Key Protocol:**
    * **BGP (Border Gateway Protocol):** The only EGP currently in widespread use. Path Vector protocol. The core routing protocol of the Internet.

#### 5. Summary Comparison

| Criterion | IGP (OSPF, EIGRP) | EGP (BGP) |
| :--- | :--- | :--- |
| **Operating Scope** | **Within a single AS** | **Between different ASes** |
| **Primary Goal** | Finding the **Optimal Path** (Performance) | **Policy Control** and Inter-AS connection (Boundary) |
| **Information** | Detailed routes to all networks inside the AS | Reachability information for networks in other ASes |
| **Example** | Corporate internal network, Campus network | The entire Internet, Connections between ISPs |