---
title: "CI/CD Pipeline"
layout: single
date: 2026-01-08
categories:
  - IT Infrastructure Engineering
tags:
  - Deployment
  - Automation
use_math: true
---

## 1. High-Level Workflow
CI/CD is a method to frequently deliver apps to customers by introducing automation into the stages of app development. It bridges the gaps between development and operation teams by automating the building, testing, and deployment of applications.

*   **Continuous Integration (CI):** Focuses on the early stages of the pipeline where code changes are automatically prepared, built, and tested.
*   **Continuous Delivery/Deployment (CD):** Focuses on the latter stages where the validated code is automatically stored in a repository and then deployed to the production environment.


## 2. Detailed Tool Explanation

### 1. GitHub (Version Management System)
* **Analogy:** The Blueprint Library
* **Role:** This is where the **Source Code** lives.
* **Process:** * Developers (Architects) draw up the plans for the software.
    * They store these plans in GitHub to ensure they don't get lost or overwritten.
    * **Action:** When a plan is finished, GitHub "rings a bell" (Webhook) to wake up the manager (Jenkins).

### 2. Jenkins (Build/Deploy Tool)
* **Analogy:** The Factory Manager
* **Role:** The **Orchestrator** that controls the entire timeline.
* **Process:** * Jenkins doesn't build or move things itself; it shouts orders to other tools.
    * **Step A (Build Phase):** It sees the new blueprint in GitHub and tells **Buildah** to start assembling.
    * **Step B (Deploy Phase):** Once the product is ready and stored, it tells **Helm** to deliver it to the final location.

### 3. Buildah (Image Build)
* **Analogy:** The Assembly Robot
* **Role:** Creating the **Container Image**.
* **Process:** * Jenkins gives Buildah the blueprints (Source Code).
    * Buildah assembles the software and wraps it into a sealed, standard shipping box called a **Container Image**.
    * **Why?** This "box" ensures the software works the same way everywhere.

### 4. Harbor (Image Repository)
* **Analogy:** The Secure Warehouse
* **Role:** Storing the Artifacts (**Images** and **Charts**).
* **Process:** * **Image Registry:** The shelves where the finished boxes (Container Images from Buildah) are stored.
    * **Helm Chart Repository:** The filing cabinet that holds the "Instruction Manuals" (Helm Charts) on how to install the boxes.
    * Harbor keeps these safe until they are needed for delivery.

### 5. Helm (Kubernetes Deploy)
* **Analogy:** The Moving & Installation Crew
* **Role:** Managing the **Deployment** to Kubernetes.
* **Process:** * Jenkins gives the order to "Ship it."
    * Helm goes to the Warehouse (**Harbor**).
    * It picks up the specific Box (Image) and the Instruction Manual (Helm Chart).
    * It travels to the destination (Kubernetes) and unpacks the software, setting it up exactly according to the instructions.
