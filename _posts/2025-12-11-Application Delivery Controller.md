---
title: "Application Delivery Controller"
layout: single
date: 2025-12-11
categories:
  - IT Infrastructure Engineering
tags:
  - Network
use_math: true
---

* **Application Delivery Controller (ADC)** is a network device (physical or virtual) that resides between the client and the server farm to manage, optimize, and secure application traffic.
* **Core Functions**:
    * **Load Balancing**: Distributes incoming network traffic to ensure no single server bears too much load.
    * **Health Monitoring**: Periodically checks the status of backend servers.
    * **Optimization**: Uses caching and compression to speed up application delivery.
    * **Security**: Acts as a WAF (Web App Firewall) to protect against attacks.

### Secure Sockets Layer (SSL)
* **Definition**: A cryptographic protocol designed to provide communications security over a computer network.
* **Key Concepts**:
    * **Encryption**: Scrambles data to prevent eavesdropping.
    * **Handshake**: The authentication process between client and server.
    * **SSL Offloading**: The ADC handles the heavy decryption work so the backend servers don't have to.

### What is NetScaler?
* **Definition**: NetScaler is a specific product line of Application Delivery Controllers (originally by Citrix, now part of Cloud Software Group).
* **Role**: It functions as a comprehensive ADC, performing Load Balancing, Content Switching, and Global Server Load Balancing (GSLB).
* **Connection to SSL**: NetScaler is particularly renowned for its high-performance **SSL Offloading** capabilities. It uses dedicated hardware (SSL chips) or optimized software to terminate SSL connections, inspecting and securing traffic before sending it to backend servers.

### NetScaler Form Factors
NetScaler software is available in several deployment types, or "form factors," allowing you to use the same features whether you are on-premise, in the cloud, or using containers.

#### MPX (Multi-Platform Experience)
* **Type**: Physical Hardware Appliance.
* **Context**: The "M" stands for **Multi-Platform** (or historically, **Maximum Performance**).
* **Why this name?**: It is the "heavy metal" option—a powerful, single-tenant box designed to handle multiple features at the highest possible throughput.

#### SDX (Service Delivery Experience)
* **Type**: Multi-Tenant Hardware Appliance (Virtualization support).
* **Context**: The "S" stands for **Service Delivery**.
* **Why this name?**: It focuses on *delivering services* to multiple tenants. It takes the "Metal" (MPX) and slices it up using a hypervisor so you can run many isolated instances on one box.

#### VPX (Virtual Platform Experience)
* **Type**: Virtual Machine (Software).
* **Context**: The "V" simply stands for **Virtual**.
* **Why this name?**: It is the software version of the appliance that runs on your existing hypervisors (ESXi, Hyper-V) or in the cloud (AWS, Azure), decoupled from specific hardware.

#### CPX (Container Platform Experience)
* **Type**: Containerized Appliance (Docker).
* **Context**: The "C" stands for **Container**.
* **Why this name?**: It is a lightweight version designed for microservices, living inside container orchestration platforms like Kubernetes.

#### BLX (Bare-Metal Linux Experience)
* **Type**: Software for Linux (No Hypervisor).
* **Context**: The "B/L" stands for **Bare-Metal Linux**.
* **Why this name?**: It runs "bare" on a standard Linux OS (like Red Hat or Ubuntu) without a hypervisor layer. This allows you to use your own commodity hardware but get performance closer to a dedicated appliance.
