---
title: "Load Balancer and Clustering for High Availability"
layout: single
date: 2025-11-08
categories:
  - IT Infrastructure Engineering
tags:
  - Load balancing
  - Availability
use_math: true
---

### 1. Load Balancer

* **Primary Purpose:** Load Balancing
* **HA Role:**
    * **Health Check:** Periodically checks backend server status.
    * **Failover:** On server failure detection, immediately excludes it from traffic distribution. Routes traffic only to healthy servers.
* **Scope:** Typically within a single data center's server pool.

---

### 2. Clustering

* **Primary Purpose:** High Availability (HA) and State Sharing
* **Definition:** Technology that groups multiple servers (nodes) into a single logical unit, acting as one system.
* **Key Function:** Detects failures via inter-node Heartbeats (status monitoring) and transfers roles.

#### A. Active/Standby (Active/Passive)

* **Configuration:** One node is **Active** (handling services). The other node is **Standby** (monitoring the Active node).
* **HA Operation:** On Active node failure, the Standby node immediately transitions to Active, taking over the service IP and resources (Takeover).
* **Characteristic:** Lower resource efficiency (Standby node is idle). Clear role transition upon failure.

#### B. Active/Active

* **Configuration:** **All nodes** in the cluster are simultaneously Active, handling services.
* **HA Operation:** (Usually combined with a Load Balancer) On node failure, the load balancer excludes it and distributes traffic to the remaining healthy nodes.
* **Characteristic:** Maximizes resource efficiency. Achieves load balancing simultaneously. However, total capacity decreases during a failure.

---

### 3. GSLB (Global Server Load Balancing)

* **Primary Purpose:** Disaster Recovery (DR) and Global Performance Optimization
* **HA Role:**
    * **Health Check:** Checks status at the **Data Center (DC) level**, not individual servers.
    * **Failover (Disaster Recovery):** On a full DC failure (e.g., Seoul DC), GSLB detects it. Automatically redirects all user traffic to a healthy DC (e.g., California DC).
* **Scope:** Between geographically distributed data centers.
* **Mechanism:** Operates primarily via DNS queries. Responds with the IP of the closest or healthiest data center to the user.
