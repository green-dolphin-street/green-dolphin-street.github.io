---
title: "EFK and PLG Stacks"
layout: single
date: 2026-01-09
categories:
  - IT Infrastructure Engineering
tags:
  - Monitoring
  - Automation
use_math: true
---

## EFK and PLG Stacks
Monitoring and observability are critical for managing modern distributed systems. Two of the most popular open-source solutions for log aggregation and analysis are the **EFK (Elasticsearch, Fluentd, Kibana)** stack and the **PLG (Promtail, Loki, Grafana)** stack.

While both stacks aim to provide centralized logging, they differ significantly in their architecture and resource requirements:
*   **EFK Stack:** Uses **Elasticsearch** as its core, which indexes the full text of every log message. This allows for powerful, complex searches but requires significant CPU and memory resources.
*   **PLG Stack:** Uses **Loki**, which takes a "Prometheus-inspired" approach. Instead of indexing the full log content, Loki only indexes metadata (labels). This makes the PLG stack much more lightweight and cost-effective, though it may be slower for full-text searches across massive datasets compared to Elasticsearch.

In short, **EFK** is often preferred for deep data analysis and complex querying, while **PLG** is favored for high-efficiency, cloud-native environments where resource optimization is a priority.


## EFK Stack: End-to-End Logging Pipeline

This section outlines the architecture of the EFK stack (Elasticsearch, Fluentd, Kibana), detailing how log data is captured, processed, stored, and visualized.

### 1. The Source: Log Generation
Before the stack gets involved, applications and systems must generate "diaries" of what they are doing.

* **Application/System Logs**
    * **What it is:** The raw text output from your servers, containers, or code (e.g., "User logged in," "Database connection failed").
    * **Role in Pipeline:** The raw material. Without these text files, the EFK stack has nothing to read.

### 2. The Collection & Transport Layer: "The Courier"
This is the "F" in EFK. It is responsible for picking up the logs and moving them.

* **Fluentd (or Fluent Bit)**
    * **What it is:** A unified logging layer. Think of it as a smart courier or a data pipe.
        * *Fluent Bit:* Often used as a lightweight "agent" on every server to collect logs locally.
        * *Fluentd:* Often used as a central "aggregator" to receive logs from Fluent Bit, filter them, and sort them.
    * **Role in Pipeline:** It tails (reads) the log files, converts them into a structured format (JSON), and ships them to the database. It handles the messy work of parsing different log formats.

### 3. The Storage & Indexing Layer: "The Library"
This is the "E" in EFK. It solves the problem of searching through millions of lines of text instantly.

* **Elasticsearch**
    * **What it is:** A distributed search and analytics engine. It is a NoSQL database specialized in text search.
    * **Role in Pipeline:** It receives the structured logs from Fluentd and "indexes" every word. This allows you to search for a specific error code across terabytes of data in milliseconds.

### 4. The Visualization Layer: "The Interface"
This is the "K" in EFK. It is the window through which humans interact with the data.

* **Kibana**
    * **What it is:** A browser-based user interface designed specifically for Elasticsearch.
    * **Role in Pipeline:** It queries Elasticsearch to build dashboards, pie charts, and lists. It allows you to filter logs (e.g., "Show me only 'Critical' errors from the last 15 minutes").

---

### Workflow Summary

1.  **Generate:** Your App writes a line of text: `[Error] Database timeout`.
2.  **Collect:** **Fluentd** reads this line, tags it with the time and server name, and converts it to JSON.
3.  **Store:** Fluentd sends this JSON to **Elasticsearch**, which saves it and updates its search index.
4.  **Visualize:** You open **Kibana**, type "timeout", and it instantly shows you that specific log line and a graph of how often it happened.




## PLG Stack: The Cloud-Native Logging Pipeline

This section outlines the architecture of the PLG stack (Promtail, Loki, Grafana). It is famous for being much cheaper and more efficient than EFK because it doesn't try to index every single word—only the important labels.

### 1. The Source: Log Generation
Just like with EFK, the process starts with your applications or servers writing text to a file or output stream.

* **Standard Output (stdout/stderr)**
    * **What it is:** The standard way containers "speak." When your app prints "User logged in," it goes here.
    * **Role in Pipeline:** The raw evidence of what is happening inside your containers.

### 2. The Collection Layer: "The Agent"
This is the "P" in PLG. It replaces Fluentd/Logstash.

* **Promtail**
    * **What it is:** A tiny, lightweight agent installed on every server (Node).
    * **Role in Pipeline:** It "tails" (reads) the log files on the server.
    * **Crucial Difference:** Unlike Fluentd, it doesn't try to understand the *content* of the log. It just tags it with labels (e.g., `app=frontend`, `env=prod`) and ships it immediately. It uses the exact same labeling system as Prometheus.

### 3. The Storage Engine: "The Efficient Archive"
This is the "L" in PLG. It is the heart of the system.

* **Loki**
    * **What it is:** A log aggregation system inspired by Prometheus.
    * **Role in Pipeline:** It receives the logs from Promtail.
    * **The Secret Sauce:** Instead of building a massive search index for every word (like Elasticsearch), Loki **only indexes the labels** (metadata). It compresses the actual log text and stores it in cheap object storage (like S3).
    * **Result:** It uses ~90% less storage and RAM than Elasticsearch, but it relies on you knowing the right tags (labels) to find things.

### 4. The Visualization Interface: "The Single Pane of Glass"
This is the "G" in PLG.

* **Grafana**
    * **What it is:** The same dashboard tool used for metrics (Prometheus).
    * **Role in Pipeline:** You use the **LogQL** query language (which looks just like PromQL) to search logs.
    * **Integration:** Because it's the same tool as your metrics, you can split your screen: Top half shows "CPU High" (Prometheus), bottom half shows the exact Error Logs from that same second (Loki).

---

### Workflow Summary

1.  **Generate:** Your App prints: `[Error] Payment failed`.
2.  **Label & Ship:** **Promtail** sees this, tags it with `{app="payment", container="pod-1"}`, and shoots it to Loki.
3.  **Compress & Store:** **Loki** groups this log with others from the same app, compresses them into a tiny "chunk," and throws it into storage (S3). It records: "Logs for 'payment' app are in Chunk #502."
4.  **Visualize:** You open **Grafana**, select "Payment App," and it instantly fetches Chunk #502, unzips it, and shows you the error.
