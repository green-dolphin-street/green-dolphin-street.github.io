---
title: "Virtual Routing and Forwarding (VRF) and Bridging"
layout: single
date: 2025-11-08
categories:
  - IT Infrastructure Engineering
tags:
  - Network
use_math: true
---

#### 1. VRF (Virtual Routing and Forwarding)

* **Concept:** A technology that logically separates the **Routing Layer (L3)** within a single physical router, making it function as multiple independent **Virtual Routers**.
* **Operational Characteristics:**
    * Each VRF maintains its own separate **Routing Table, ARP Table, and interface set**.
    * Traffic between different VRFs is fundamentally isolated and cannot communicate by default (requires explicit route leaking or external routing).
* **Primary Use Cases:** MPLS VPN for customer separation, environments requiring strict security segmentation (e.g., Guest vs. Internal networks).

#### 2. Bridging

* **Concept:** A technology that connects two or more physical network segments at the **Data Link Layer (L2)**, effectively merging them into **one logical Broadcast Domain**.
* **Operational Characteristics:**
    * Performed primarily by **Switches** or **Bridges**.
    * Forwards frames based on **MAC Addresses (L2)**.
    * All connected devices share the same IP subnet, and broadcast traffic reaches all devices within the domain.
* **Primary Use Cases:** Communication within the same subnet, segmenting traffic within a single VLAN.
