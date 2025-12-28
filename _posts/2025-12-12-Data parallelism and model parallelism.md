---
title: "Data Parallelism and Model Parallelism"
layout: single
date: 2025-12-12
categories:
  - IT Infrastructure Engineering
tags:
  - AI
use_math: true
---

### 1. Data Parallelism (DP)
* **Core Concept:** Spreading the data across multiple devices while keeping the model identical on each.
* **The Workflow:**
    * **Replication:** A complete copy of the model parameters is loaded onto every GPU (worker).
    * **Distribution:** The global training dataset is split into mini-batches.
    * **Parallel Processing:** Each GPU processes a different mini-batch simultaneously.
    * **Synchronization (All-Reduce):**
        * After the backward pass, gradients are calculated locally.
        * Gradients are averaged across all GPUs.
        * Weights are updated so all model copies remain mathematically identical.
* **Primary Bottleneck:** Network bandwidth required to synchronize gradients (All-Reduce step).
* **Best Use Case:** When the model fits in memory, but the dataset is large and you want to reduce training time.

### 2. Model Parallelism (MP)
* **Core Concept:** Splitting the model itself across multiple GPUs because it is too large to fit in a single device's VRAM.
* **Type A: Pipeline Parallelism (Inter-layer)**
    * **Method:** The model layers are vertically sliced.
    * **Example:** GPU 1 holds Layers 1-10; GPU 2 holds Layers 11-20.
    * **Flow:** Data moves sequentially from GPU 1 -> GPU 2.
    * **Optimization:** Uses "Micro-batches" to keep GPUs busy and reduce the "bubble" (idle time).
* **Type B: Tensor Parallelism (Intra-layer)**
    * **Method:** Specific heavy operations (like Matrix Multiplication) are horizontally sliced.
    * **Example:** In a Transformer, the Attention Head calculations (Q, K, V projections) are split across GPUs.
    * **Flow:** GPUs calculate partial results and communicate immediately to combine them for the final layer output.
* **Primary Bottleneck:** extremely high latency sensitivity; requires fast communication between specific operations.

### 3. How It Is Achieved (The Tech Stack)
* **Hardware Layer (The "Pipes")**
    * **High-Bandwidth Memory (HBM):** Essential for storing the massive parameters of LLMs (e.g., A100/H100 80GB).
    * **NVLink / NVSwitch:** Proprietary NVIDIA interconnects allowing GPUs to talk at ~900GB/s (bypassing the slower CPU/PCIe).
    * **InfiniBand:** High-speed networking used to connect different server nodes in a cluster.

* **Software Layer (The "Logic")**
    * **PyTorch DDP (Distributed Data Parallel):** The industry standard for basic Data Parallelism.
    * **FSDP (Fully Sharded Data Parallel):**
        * Shards parameters, gradients, and optimizer states across GPUs.
        * Allows training distinctively larger models than standard DDP by saving memory.
    * **DeepSpeed (by Microsoft):**
        * Implements "ZeRO" (Zero Redundancy Optimizer).
        * Can offload optimizer states to the CPU/RAM (NVMe offloading) to train massive models on limited GPU resources.
    * **Megatron-LM:** Specialized for Tensor Parallelism to train giant Transformer models (like GPT).

### 4. Summary Comparison
* **Data Parallelism:** Split the **Batch**. (Focus: Speed)
* **Pipeline Parallelism:** Split the **Layers**. (Focus: Memory/Scale)
* **Tensor Parallelism:** Split the **Math Operations**. (Focus: Memory/Scale)
* **3D Parallelism:** Combining all three methods simultaneously for training Trillion-parameter models.
