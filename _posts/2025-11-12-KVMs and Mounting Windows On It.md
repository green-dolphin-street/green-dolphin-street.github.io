---
title: "KVMs and Mounting Windows On It"
layout: single
date: 2025-11-12
categories:
  - IT Infrastructure Engineering
tags:
  - OS
  - Virtualization
  - Resource management
use_math: true
---

### Kernel-based Virtual Machine (KVM)

* KVM is an open-source virtualization technology that is **built directly into the Linux kernel**.
* **Type 1 Hypervisor:** It effectively turns the Linux kernel itself into a "bare-metal" hypervisor. It runs virtual machines as standard Linux processes, scheduled by the kernel, giving it direct access to hardware and high efficiency.
* **Hardware Needs:** It requires a CPU with virtualization extensions, such as **Intel VT-x** or **AMD-V**.
* **Pairing with QEMU:** KVM is almost always used with **QEMU**. KVM handles the high-speed CPU and memory virtualization, while QEMU emulates the "machine" (virtual motherboard, disk controllers, network cards, etc.).
* **Guest Support:** It can run almost any unmodified guest operating system, including all modern versions of Windows, Linux, and BSD.

---

### Using Windows on KVM for HPC Visualization

* This is a very common and practical solution in HPC environments.
* **The "Why" (Application Support):** The main HPC cluster (compute nodes) runs Linux, but end-users (scientists, engineers) often need visualization or post-processing software that is **only available for Windows**.
    * Examples include commercial CAD/CAE software (like ANSYS, SolidWorks, COMSOL), medical imaging tools, and financial modeling applications.
* **The "How" (High Performance):** To run demanding 3D or CUDA applications, you don't use emulated graphics. Instead, you use **GPU Passthrough (VFIO)**.
    * **Host Setup:** You install a powerful, data-center-grade GPU (e.g., NVIDIA A-series or RTX) into the Linux server running KVM.
    * **Passthrough:** You configure KVM to pass this *entire physical GPU* directly to a specific Windows VM.
    * **Guest VM:** The Windows VM sees the GPU as if it were physically installed. It uses the standard NVIDIA drivers, giving it **near-native graphics and compute performance**.
* **User Access:** The end-user connects to this high-performance Windows VM remotely using a protocol optimized for graphics, such as **NICE DCV**, HP Anyware, or Microsoft RDP.

