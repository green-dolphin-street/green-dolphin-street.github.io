# Hypervisor Summary

A **Hypervisor**, also known as a **Virtual Machine Monitor (VMM)**, is the foundational technology that enables virtualization by allowing multiple independent **Virtual Machines (VMs)** to run on a single physical host computer.

## Core Functions

1.  **VM Creation and Management:** Creates and controls multiple virtualized environments (guests) on one physical machine (host).
2.  **Resource Allocation:** Pools and dynamically distributes the host's physical resources (CPU, RAM, Storage, Network) among all running VMs.
3.  **Isolation:** Ensures that each VM is completely separated from others. A failure or security breach in one VM will not affect the others.
4.  **Hardware Abstraction/Emulation:** Acts as an intermediary, presenting virtualized hardware to the guest OS while translating their resource requests for the underlying physical hardware.

## Types of Hypervisors

| Feature | Type 1 (Bare-Metal/Native) | Type 2 (Hosted) |
| :--- | :--- | :--- |
| **Location** | Runs directly on the host's physical hardware. | Runs as an application *on top* of a host operating system. |
| **Intermediary** | No host OS layer between the hypervisor and hardware. | The host OS acts as a layer between the hypervisor and hardware. |
| **Performance** | Higher (Direct hardware access, low overhead). | Lower (Must pass requests through the host OS). |
| **Security** | Higher (Smaller attack surface). | Lower (Relies on the security of the host OS). |
| **Use Case** | Enterprise data centers, cloud computing (e.g., VMware ESXi, Microsoft Hyper-V, KVM). | Desktop use, development, and testing (e.g., VirtualBox, VMware Workstation). |

## Implementation and Context

The hypervisor is often described as a layer of **software, firmware, or hardware** because:

* **Software:** The hypervisor itself is a program (e.g., ESXi is specialized OS-like software; VirtualBox is an application).
* **Firmware:** Core functions can be integrated into the server's firmware (BIOS/UEFI) for stability and direct control.
* **Hardware:** It fundamentally relies on specialized **hardware-assisted virtualization** features built into the CPU (Intel VT-x, AMD-V) to operate efficiently.

In **cloud computing**, public cloud providers universally use **Type 1 (Bare-Metal) hypervisors** on their servers to offer secure, high-performance, and isolated VM instances (IaaS) to their customers.