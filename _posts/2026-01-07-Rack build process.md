---
title: ""
layout: single
date: 2026-01-07
categories:
  - IT Infrastructure Engineering
tags:
  - Automation
  - Deployment
use_math: true
---

### 1. Discovery & Validation
* **Discovery:** * Detects new MAC addresses on the management network (OOB) immediately upon plug-in.
* **Validation:** * Queries hardware via IPMI or Redfish APIs.
    * Verifies hardware matches the Bill of Materials (e.g., confirming presence of 8 GPUs or 2TB RAM).

### 2. Firmware Baseline
* **Flashing:** * Updates BIOS, BMC, NICs, and switch OS.
* **Consistency:** * Ensures all hardware is on a specific "known good" version to maintain datacenter consistency.

### 3. OS Provisioning
* **Boot Method:** * Servers boot over the network using protocols like PXE (Preboot Execution Environment) or iPXE.
* **Installation:** * Automatically installs the operating system using tools like Kickstart or Cloud-init.

### 4. Network Fabric Configuration
* **Switch Config:** * Automation logs into Top-of-Rack (ToR) switches using tools like Ansible or Terraform.
* **Settings:** * Configures VLANs, BGP routing, and port channels.

### 5. Rack Awareness
* **Metadata Injection:** * Injects location data into the servers so they know their physical position (e.g., "I am in Rack-04").
* **Topology:** * Enables cluster logic (like Hadoop or Kubernetes) to handle failures by replicating data to different physical racks.

### 6. The Build Artifact (State Management)
* **Success:** * Rack is marked "Active" in the inventory system (e.g., NetBox, CMDB) and scheduling begins.
* **Failure:** * If any step fails (e.g., PXE timeout), the node/rack is marked "Tainted" or "In Maintenance" and a human is alerted.
