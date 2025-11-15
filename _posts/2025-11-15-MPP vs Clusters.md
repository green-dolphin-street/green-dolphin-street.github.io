---
title: "'Massively Parallel Processing' vs 'Cluster' in HPC TOP500 Lists"
layout: single
date: 2025-11-15
categories:
  - IT Infrastructure Engineering
tags:
  - HPC
use_math: true
---

### MPP (Massively Parallel Processing)

* Describes a **tightly integrated, proprietary system** often designed and sold by a single vendor (e.g., Cray, Fujitsu).
* The key differentiator is a **custom, proprietary interconnect** (like Cray's Slingshot or Fujitsu's Tofu) designed for extreme low latency and high bandwidth.
* While it may use commodity processors (CPUs/GPUs), the way they are packaged into nodes and blades is part of a **custom, non-commodity design**.
* The entire system is engineered from the ground up to function as a single, cohesive parallel machine.

###  Cluster

* Refers to a system built by connecting **many independent, stand-alone servers** (nodes).
* These nodes are typically built from **commodity, off-the-shelf components** (e.g., standard x86 servers, common Linux distributions).
* The nodes are linked by a **high-speed, but standard, network** such as InfiniBand or, in some cases, high-speed Ethernet.
* It's a more "component-based" approach, assembling a supercomputer from parts that are not exclusive to that single system.
