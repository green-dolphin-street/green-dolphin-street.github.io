---
title: "Availability zone"
layout: single
date: 2025-11-08
categories:
  - IT Infrastructure Engineering
tags:
  - Availability
---

# **Availability zone**
* An availability zone (AZ) is one or more discrete, independent data centers within a specific geographic area.
* The core design principle is **fault isolation**. Each AZ has its own independent power, cooling, and networking.
* This means a physical problem like a fire, power outage, or flood that takes down one AZ will not affect the other AZs in that same region. They are physically separate buildings.

### Examples of AWS region and AZs
* A Region is the large geographic area, and the AZs are the individual facilities within it.
* Region: `us-east-1` (N. Virginia, USA)
    * Its AZs: `us-east-1a`, `us-east-1b`, `us-east-1c`, `us-east-1d`, `us-east-1e`, `us-east-1f`
* Region: `ap-northeast-2` (Seoul, South Korea)
    * Its AZs: `ap-northeast-2a`, `ap-northeast-2b`, `ap-northeast-2c`, `ap-northeast-2d`

### Are AZs in a region exact copies of each other?
* AZs in the same region are not exact copies of each other.
* Hardware: They are often built at different times. This means `us-east-1a` might have a different generation of server hardware than `us-east-1c`. The cloud provider's virtualization software is responsible for ensuring a "medium" server performs consistently, regardless of the underlying physical chip.
* Software (Cloud Services): From the customer's perspective, the services are the same. The regional control plane (the API you use to manage services) is consistent across all AZs. You have the same capability to launch a VM or database in any of them.

### How AZs are provided to application builders
* The cloud provider gives the option of using multiple AZs to the "application building customer" (e.g., an engineer at Dropbox). It is not automatic.
* Single-AZ Deployment: You can choose to run your entire application in one AZ. This is cheaper but has low resilience. If that AZ fails, your app goes offline.
* Multi-AZ Deployment: You can choose to run redundant copies of your application in two or more AZs. This is the resilient, high-availability choice. It is more expensive because:
    1.  You are paying for duplicate resources (e.g., 10 VMs in AZ1 and 10 VMs in AZ2).
    2.  You pay data transfer fees for the data you replicate between the AZs (e.g., keeping your database in AZ2 in sync with AZ1).

### The concept of AZ is abstracted from end-users
* The end-user (e.g., a person uploading a file to Dropbox) can be completely unknowing of this concept.
* This "abstraction" is the job of the application builder (Dropbox).
1. The end-user uploads a file. They simply trust Dropbox will "keep it safe."
2. The application builder (Dropbox) receives that file. To fulfill that promise of "safety," Dropbox's application is built to use the multi-AZ option.
3. Dropbox's code (or a managed service it uses, like Amazon S3) intentionally replicates that user's file to copies in AZ1, AZ2, and AZ3.
4. Result: If a fire destroys the data center for AZ1, the end-user notices nothing. When they ask for their file, Dropbox's application (which knows AZ1 is down) simply serves the copy from AZ2.
