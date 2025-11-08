---
title: "Networking Tables (MAC, ARP, etc.)"
layout: single
date: 2025-11-08
categories:
  - IT Infrastructure Engineering
tags:
  - Network
use_math: true
---

### The big picture
* **Layer 2 (L2):** Local delivery (same network). Uses **MAC addresses**. Like a mail carrier on one street.
* **Layer 3 (L3):** Remote delivery (different networks). Uses **IP addresses**. Like the main post office routing mail to another city.

---

### 1. L2 MAC Address Table (Switch Port Table)
* **Device:** Network Switch.
* **Purpose:** Maps a **MAC address -> physical Switch Port**.
* **How it learns:** From the *source* MAC address of incoming frames. (e.g., "MAC `AA:BB:CC:11:22:33` is on Port 5.")


#### Scenarios:
* **Same Subnet (PC-A -> PC-B):**
    1.  Frame arrives with PC-B's MAC as destination.
    2.  Switch looks up PC-B's MAC.
    3.  Finds: `MAC-B-Address -> Port 8`.
    4.  Action: Forwards frame *only* to Port 8.
* **Different Network (PC-A -> Server):**
    1.  Frame arrives with **Gateway's MAC** as destination.
    2.  Switch looks up the Gateway's MAC.
    3.  Finds: `Gateway-MAC-Address -> Port 20`.
    4.  Action: Forwards frame *only* to Port 20 (the router).

---

### 2. ARP Table (Address Resolution Protocol)
* **Device:** PCs, Routers (any L3-aware device).
* **Purpose:** Maps an **IP address -> MAC address** (only on the local network).
* **Why:** Need a destination MAC to build the L2 frame for local sending.
* **How it learns (if unknown):**
    * **ARP Request (Broadcast):** "Who has IP `192.168.1.20`? Tell `192.168.1.10`."
    * **ARP Reply (Unicast):** "I am `192.168.1.20`. My MAC is `BB:BB:BB:...`"

#### Scenarios:
* **Same Subnet (PC-A: `.1.10` -> PC-B: `.1.20`):**
    1.  Goal: Find MAC for `192.168.1.20`.
    2.  Action: Check ARP table. If not found, broadcast ARP Request for `.1.20`.
    3.  Result: PC-B replies. PC-A stores: `192.168.1.20 -> MAC-B`.
* **Different Network (PC-A: `.1.10` -> Server: `10.0.0.50`):**
    1.  Goal: Must send to Gateway (`192.168.1.1`). Need **Gateway's MAC**.
    2.  Action: Check ARP table for `192.168.1.1`. If not found, broadcast ARP Request for `.1.1`.
    3.  Result: Router replies. PC-A stores: `192.168.1.1 -> Gateway-MAC`.

---

### 3. Routing Table
* **Device:** PCs, Routers.
* **Purpose:** Maps a **Destination IP Network -> Next-Hop IP (Gateway)**.
* **First Decision:** "Is this local or remote?"



#### Scenarios:
* **Same Subnet (PC-A: `.1.10` -> PC-B: `.1.20`):**
    1.  PC-A checks table for `192.168.1.20`.
    2.  Match: Route for `192.168.1.0 / 255.255.255.0`.
    3.  Gateway: `On-link` (or `0.0.0.0`).
    4.  Decision: "Local. Use ARP directly for `192.168.1.20`."
* **Different Network (PC-A: `.1.10` -> Server: `10.0.0.50`):**
    1.  PC-A checks table for `10.0.0.50`.
    2.  Match: No specific route. Uses **Default Route** `0.0.0.0 / 0.0.0.0`.
    3.  Gateway: `192.168.1.1`.
    4.  Decision: "Remote. Send to Gateway `192.168.1.1`. Now, use ARP to find the *gateway's* MAC."

---

### Summary Flow (PC-A -> Server on Different Network)
1.  **PC-A (Routing Table):** Look up `10.0.0.50`.
    * Result: Remote. Use Gateway `192.168.1.1`.
2.  **PC-A (ARP Table):** Look up `192.168.1.1`.
    * Result: `Gateway-MAC` (or runs ARP request).
3.  **PC-A (Builds Data):**
    * **Packet (L3):** Dest IP: `10.0.0.50` (final destination)
    * **Frame (L2):** Dest MAC: `Gateway-MAC` (next hop)
4.  **Switch (MAC Table):** Look up `Gateway-MAC`.
    * Result: Port 20. Forwards frame to router.
5.  **Router:** Receives frame, opens packet, sees Dest IP `10.0.0.50`.
6.  **Router (Routing Table):** Repeats the process. Looks up `10.0.0.50` to find *its* next hop.
