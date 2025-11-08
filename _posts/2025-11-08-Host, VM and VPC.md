---
title: "Virtual Machines and Virtual Private Cloud"
layout: single
date: 2025-11-08
categories:
  - IT Infrastructure Engineering
tags:
  - Resource management
  - Virtualization
use_math: true
---

### 1. Host and VM are in Cloud

* **Host:** The **physical server** in the data center. It's the real hardware with the CPU, RAM, and storage. It runs a **hypervisor** (special software).
* **VM (Virtual Machine):** A **software-emulated computer** that runs as a "guest" on the host. The hypervisor "carves up" the host's physical resources (CPU, RAM) and gives a slice to each VM. Multiple isolated VMs can run on a single host.

### 2. What VPC is, and How It Spans Many Hosts

* **VPC (Virtual Private Cloud):** An **isolated, private network** that you define *inside* the cloud. It's not a physical thing; it's a logical boundary created with **Software-Defined Networking (SDN)**.
* **The Hypervisor's Role in Adding VMs:**
    1.  When you request a new VM in your VPC, the cloud's management system selects a physical **Host** with available capacity.
    2.  It instructs the **Hypervisor** on that host to create the VM.
    3.  As part of the VM's creation, the hypervisor creates a **virtual Network Interface Card (vNIC)**.
    4.  The hypervisor is then told to **logically attach** this vNIC to your specific VPC and subnet. It "plugs" the VM into your virtual network.
    5.  From then on, the hypervisor (working with the host's physical hardware) ensures all traffic from that VM's vNIC is handled by the SDN software, which enforces all your VPC's rules (security, routing, etc.).
* **How it Spans Hosts:** Because the VPC is purely software-defined, it's not tied to one box.
    * You can launch `VM-1` (on `Host-A`) and `VM-2` (on `Host-B`) into the *same VPC*.
    * The SDN "overlay" automatically creates a virtual tunnel between them, making them seem like they're on the same local network, even if the hosts are in different racks.

### 3. How Different Parties Use VPCs and For What

* **SaaS Companies & Application Developers:**
    * **Purpose:** To build secure, **multi-tier applications**.
    * **Example:** A 3-tier app.
        * **Public Subnet:** Holds the web servers (the only part the internet sees).
        * **Private "App" Subnet:** Holds the application logic. Only the web servers can talk to it.
        * **Private "DB" Subnet:** Holds the database. Only the app servers can talk to it. This provides extreme security.

* **Enterprise IT:**
    * **Purpose 1: Hybrid Cloud.** To securely connect their on-premises data center to the cloud (using a VPN or direct fiber line), making the VPC an extension of their private office network.
    * **Purpose 2: "Lift and Shift."** To move old, internal applications onto VMs in the cloud without having to re-architect them, placing them in a familiar, private network space.

* **Data Scientists & Researchers:**
    * **Purpose:** To create a secure "sandbox" for analysis.
    * **Example:** They can load sensitive financial or medical data into a VPC that is completely sealed off from the public internet, run powerful AI/ML models on it, and extract the results, all without risk of exposure.
