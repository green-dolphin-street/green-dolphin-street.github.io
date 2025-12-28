---
title: "Confidential Computing Basics"
layout: single
date: 2025-12-23
categories:
  - IT Infrastructure Engineering
tags:
  - Security
use_math: true
---


# Confidential Computing & GPU Security Overview

### 1. What is Confidential Computing?
Confidential Computing is a security technology that protects **Data in Use**. 

While traditional security measures protect data while it is being stored or transmitted, Confidential Computing protects data **while it is being processed in memory (RAM/VRAM)**. It creates a hardware-based **Trusted Execution Environment (TEE)**, often called an "Enclave," which isolates the data and code from the operating system, hypervisor, and other privileged software.

**Why it matters:** It ensures that even if a hacker gains root access to the host server or the cloud provider is compromised, they cannot view the data inside the TEE.

---

### 2. Comparison with Traditional Security Layers

| State | Security Focus | Typical Technology |
| :--- | :--- | :--- |
| **Data at Rest** | Protecting data stored on disk | AES-XTS, Self-Encrypting Drives (SED) |
| **Data in Transit** | Protecting data moving over the network | TLS, SSH, IPsec, MACsec |
| **Data in Use** | **Protecting data in RAM/Compute** | **Confidential Computing (TEE)** |

---

### 3. Key Concepts in GPU Confidential Computing

#### GPU Attestation (Remote Attestation)
* **Definition:** A cryptographic challenge-response protocol to verify the authenticity and integrity of the GPU hardware and firmware.
* **Process:**
    1. The Verifier (a trusted external server) challenges the GPU.
    2. The GPU generates a signed "Evidence" report using a unique key burned into the silicon during manufacturing.
    3. The Verifier checks the signature against the manufacturer's (e.g., NVIDIA's) root of trust.
* **Goal:** To prove the GPU is genuine, running secure firmware, and has Confidential Compute Mode enabled *before* sending any sensitive data/keys to it.

#### NVML (NVIDIA Management Library)
* **Standard Role:** A C-based API for monitoring GPU status (utilization, temperature, fan speed).
* **Role in CC:** In the context of Confidential Computing, NVML is extended to act as the **interface for fetching Attestation Evidence**. It allows the user space application to request the signed security report from the GPU driver/firmware.

#### P-PCIe (Protected PCIe) / IDE (Integrity and Data Encryption)
* **Definition:** A mechanism to encrypt traffic flowing across the physical PCIe bus between the CPU and GPU.
* **Threat Model:** Protects against physical attacks (interposing hardware sniffers on the bus) or compromised hypervisors trying to snoop on data transfers.
* **Requirement:** Requires strict synchronization (handshake) between the GPU firmware and the CPU kernel driver.

---

### 4. Hardware Requirements (NVIDIA Ecosystem)

To enable a fully encrypted "Data in Use" pipeline for HPC/AI, specific hardware generations are required.

#### GPU Requirements
* **Supported:** * **NVIDIA H100 (Hopper):** First generation to support APM (Architecture Protected Mode) and full VM-based Confidential Computing.
    * **NVIDIA B200 (Blackwell):** Second generation; improved encryption throughput and larger enclave support.
* **Not Supported:** * **NVIDIA A100 (Ampere):** Supports Secure Boot but lacks the hardware engines for real-time memory encryption and TEE isolation.

#### CPU Requirements
The host CPU must also support TEE technologies to establish the initial "Trust Chain" with the GPU.
* **AMD:** EPYC Genoa (Zen 4) or newer with **SEV-SNP** (Secure Encrypted Virtualization - Secure Nested Paging).
* **Intel:** Sapphire Rapids (4th Gen Xeon) or newer with **TDX** (Trust Domain Extensions).
* **Note:** Older CPUs (AMD Milan, Intel Ice Lake) cannot act as a Confidential Computing host for H100s, even if the GPU supports it.


