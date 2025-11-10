---
title: "L2, L3, L4, L7 Switches and Routers"
layout: single
date: 2025-11-10
categories:
  - IT Infrastructure Engineering
tags:
  - Network
use_math: true
---

### Differences at a Glance

* **L2 Switch (Layer 2 - Data Link)**
    * **What it reads:** MAC Addresses
    * **What it does:** Forwards data (frames) *within* a single network (like one office VLAN).
    * **Decision:** "Which physical port is this hardware address (`AA:BB:CC...`) connected to?"
    * **Key Tech:** CAM Table

* **Router (Layer 3 - Network)**
    * **What it reads:** IP Addresses
    * **What it does:** Connects *different* networks together (e.g., your office to the internet).
    * **Decision:** "What is the best path to get this packet (`172.217.14.228`) to its destination network?"
    * **Key Tech:** Routing Table, often uses software/NPUs for complex features (NAT, VPN).

* **L3 Switch (Layer 3 - Network)**
    * **What it reads:** IP Addresses
    * **What it does:** A high-speed router, optimized to route traffic *inside* a large campus or data center (e.g., between VLAN 10 and VLAN 20).
    * **Decision:** Same as a router, but performs the IP lookup in specialized hardware (ASICs) for maximum speed.
    * **Key Tech:** TCAM (Hardware Routing Table)

* **L4 Switch (Layer 4 - Transport)**
    * **What it reads:** IP Addresses + **Port Numbers** (TCP/UDP)
    * **What it does:** Acts as a basic load balancer. It understands *services*, not just devices.
    * **Decision:** "This is web traffic (Port 443). I'll send it to the least busy web server."
    * **Key Tech:** Session Table

* **L7 Switch (Layer 7 - Application)**
    * **What it reads:** The actual **application data** (e.g., HTTP URLs, cookies, headers).
    * **What it does:** An advanced load balancer or Application Delivery Controller (ADC). It understands the *content* of the traffic.
    * **Decision:** "This is a request for `/api`, send it to the API server pool. This is for `/images`, send it to the cache servers."
    * **Key Tech:** Full-proxy (terminates connections)
