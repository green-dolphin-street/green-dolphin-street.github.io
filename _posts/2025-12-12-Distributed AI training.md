---
title: "Distributed AI Training: Communication Patterns"
layout: single
date: 2025-12-12
categories:
  - IT Infrastructure Engineering
tags:
  - AI
  - HPC
use_math: true
---

### Context: Distributed Learning & MPI
* **Background**: As deep learning models (e.g., GPT-3, Megatron-Turing NLG) and datasets grow larger, single-node training becomes insufficient. Distributed learning across multiple nodes is required.
* **The Challenge**: In distributed learning (specifically Data Parallelism), each GPU/process calculates its own gradients based on a subset of data. To update the model correctly, these gradients must be averaged across all nodes.
* **MPI (Message Passing Interface)**: A standard d2fined for exchanging information between processes in concurrent computing environments. It defines how processes (identified by "Ranks") exchange messages (Data, Tag, etc.).

### 1:N Communication Patterns
* **Broadcast**
    * **Definition**: A single process (Root) sends the exact same copy of data to all other processes.
    * **Use Case**: Sending initial model parameters or configuration to all workers.
* **Scatter**
    * **Definition**: A single process divides a dataset into chunks and sends a different chunk to each process.
    * **Use Case**: Distributing a large batch of data across multiple GPUs for processing.
* **Gather**
    * **Definition**: The inverse of Scatter. A single process collects different chunks of data from all other processes and reassembles them into a single array.
    * **Use Case**: Collecting results or logs from all workers to a master node.
* **Reduce**
    * **Definition**: Similar to Gather, but instead of just collecting the raw data, it aggregates them using a specific operation (e.g., Sum, Max, Min). The result is stored only on the root process.
    * **Use Case**: calculating a global sum or finding the maximum value across all nodes.

### N:N Communication Patterns
* **All-Gather**
    * **Definition**: Every process scatters its own data to all other processes. By the end, every process has a copy of the gathered data from everyone else.
    * **Difference**: Unlike Gather (where result is on one node), here the result is on *all* nodes.
* **All-Reduce**
    * **Definition**: Performs a Reduction (e.g., Sum) on data from all processes and distributes the final aggregated result to all processes.
    * **Importance**: This is the critical pattern for distributed deep learning. It ensures every GPU ends up with the same averaged gradients to update their model weights synchronously.

### Implementation Optimization: Ring-AllReduce
* **Concept**: A specific algorithm to implement All-Reduce efficiently to minimize communication bottlenecks.
* **Mechanism**:
    * Processes are arranged in a logical ring.
    * Data is split into $P$ chunks (where $P$ is the number of processes).
    * **Scatter-Reduce Step**: Chunks are passed around the ring. As a chunk passes through a node, that node adds its own value to it. After $P-1$ steps, every chunk is fully reduced (summed).
    * **All-Gather Step**: The fully reduced chunks are circulated around the ring again so that every node receives the complete final result.
* **Benefit**: Optimizes bandwidth usage by ensuring that all nodes are sending and receiving data simultaneously, avoiding the bottleneck of a single central server.
