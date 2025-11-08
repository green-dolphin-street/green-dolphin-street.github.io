---
title: "Elastic Load Balancer (ELB) and AutoScaling"
layout: single
date: 2025-11-08
categories:
  - IT Infrastructure Engineering
tags:
  - Load balancing
  - Resource management
use_math: true
---

### Elastic Load Balancer (ELB)
* A service that automatically distributes incoming traffic across multiple target servers. It's "elastic" because the load balancer *itself* scales its own capacity up and down to match traffic levels.
* ELB Types: "ELB" is the family name for the service. The specific products are:
    * **ALB (Application Load Balancer):** Layer 7 (HTTP/HTTPS). Smart, content-based routing (e.g., by URL path). Ideal for modern apps and microservices.
    * **NLB (Network Load Balancer):** Layer 4 (TCP/UDP). Extremely fast and high-performance. Ideal for high-throughput or low-latency applications.
    * **CLB (Classic Load Balancer):** Legacy (Layer 4/7). The older generation, not recommended for new applications.

### Auto Scaling
* A service that automatically *adds or removes servers* (e.g., EC2 instances) from a resource pool of VMs based on demand (like CPU usage).
* Scaling Terminology:
    * **Horizontal Scaling (Out/In):** Changing the *number* of servers.
        * **Scale Out:** Adding more servers.
        * **Scale In:** Removing servers.
    * **Vertical Scaling (Up/Down):** Changing the *power* of a single server.
        * **Scale Up:** Increasing resources (e.g., more CPU/RAM).
        * **Scale Down:** Decreasing resources.
### ELB vs. Auto Scaling Distinction
    * **ELB (The "Traffic Cop"):** Its job is to **distribute traffic**. It does *not* add or remove servers; it just routes requests to the healthy servers in its list.
    * **Auto Scaling (The "Resource Manager"):** Its job is to **manage the server count**. It adds (scales out) or removes (scales in) VM instances from a group.
    * **They are separate services** that work together: Auto Scaling adds/removes servers, and the ELB automatically detects these changes and adjusts its traffic distribution accordingly.
