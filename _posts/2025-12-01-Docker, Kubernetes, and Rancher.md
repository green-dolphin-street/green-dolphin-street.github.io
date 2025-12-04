---
title: "Docker, Kubernetes, and Rancher"
layout: single
date: 2025-12-01
categories:
  - IT Infrastructure Engineering
tags:
  - Kubernetes
use_math: true
---


### 1. Docker & Containers (The Base Layer)
* **Container**:
    * A lightweight, standalone, executable package of software.
    * It includes everything needed to run an application: code, runtime, system tools, system libraries, and settings.
    * *Analogy*: A shipping container that fits on any ship, truck, or train without modification.
* **Docker**:
    * The most popular platform for building, sharing, and running containers.
    * **Docker Image**: The read-only template used to create containers (the "blueprint").
    * **Docker Runtime (Engine)**: The software that actually runs the container on a computer.

---

### 2. Kubernetes / K8s (The Orchestrator)
Kubernetes is the system that manages thousands of containers across many different machines.

#### A. The Cluster (The Whole Factory)
* A set of machines (Nodes) that run containerized applications.
* It consists of at least one **Control Plane** and one or more **Worker Nodes**.

#### B. Control Plane (The Brain)
The machine(s) responsible for managing the cluster's state and making global decisions.
* **API Server**: The "Front Desk." All communications (internal or external) go through here.
* **Scheduler**: The "Dispatcher." It decides which Worker Node should run a new Pod based on available resources.
* **etcd**: The "Memory." A database that stores all cluster data and configuration.
* **Controller Manager**: The "Fixer." It notices when the actual state doesn't match the desired state (e.g., a Pod crashed) and tries to fix it.

#### C. Worker Node (The Muscle)
The physical or virtual machine that performs the work.
* **Kubelet**: The "Foreman." An agent that talks to the Control Plane and ensures the Pods on this node are healthy.
* **Kube-proxy**: The "Traffic Cop." Handles network rules and traffic routing inside the node.
* **Container Runtime**: The "Engine." The software (e.g., containerd, Docker Engine) installed on the node that actually starts and stops the containers.

#### D. Pod (The Wrapper)
* The **smallest deployable unit** in Kubernetes.
* **What it is**: A logical wrapper that encapsulates one or more containers.
* **Shared Resources**:
    * **Network**: All containers in a Pod share the same IP address and can talk via `localhost`.
    * **Storage**: They share the same storage volumes.
* **Container**: The actual Docker application running inside the Pod.
    * *Note*: 1 Pod usually contains 1 Container, but can contain helper containers (sidecars).

---

### 3. Rancher (The Management Platform)
A software stack that sits *on top* of Kubernetes to make it easier to use, especially for enterprises.

#### A. Web UI (The Dashboard)
* **Vanilla K8s**: Primarily relies on a complex Command Line Interface (`kubectl`) or basic dashboards.
* **Rancher**: Provides a rich, user-friendly graphical interface to click-and-manage workloads, view logs, and monitor health without memorizing commands.

#### B. Multi-Cluster Management
* **Vanilla K8s**: You manage each cluster individually.
* **Rancher**: You can manage **multiple** Kubernetes clusters (e.g., one on AWS, one on-premise, one for Dev) from a single pane of glass.

#### C. Projects (Rancher Exclusive Feature)
* **Namespace (K8s Native)**: A way to isolate resources (like a folder).
* **Project (Rancher)**: A higher-level group that contains **multiple Namespaces**.
    * Allows you to apply rules (policies, quotas) to a whole group of namespaces at once.
    * *Hierarchy*: Cluster -> **Project** -> Namespace -> Pod.

#### D. Centralized RBAC (Role-Based Access Control)
* **Vanilla K8s**: You must configure user permissions separately for every single cluster.
* **Rancher**: You define users (via GitHub, Active Directory, etc.) in one place and assign them permissions across **all** your clusters instantly.
