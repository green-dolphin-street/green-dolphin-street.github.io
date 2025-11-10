---
title: "Database Schema"
layout: single
date: 2025-11-10
categories:
  - IT Infrastructure Engineering
tags:
  - Database
use_math: true
---


* **Definition:** A database schema is the "blueprint" of a database. It defines the structure, organization, and rules for the data, but not the data itself.
* **Key Components:**
    * **Tables:** Hold the data (e.g., `Users`).
    * **Fields (Columns):** Define the attributes within a table (e.g., `Username`, `Email`).
    * **Data Types:** Specify the type of data for each field (e.g., `INTEGER`, `VARCHAR`).
    * **Relationships:** Links between tables, often using **Primary Keys** and **Foreign Keys**.
    * **Constraints:** Rules to ensure data integrity (e.g., `NOT NULL`, `UNIQUE`).
* **Three Levels of Schema:**
    1.  **Conceptual:** The high-level view (what data is needed).
    2.  **Logical:** The detailed plan of tables and relationships (how data is organized).
    3.  **Physical:** The low-level implementation (how data is physically stored on disk).
* **Schema in SQL vs. NoSQL:**
    * **SQL (Relational):** Uses a strict **schema-on-write**, where the structure must be defined *before* data is added.
    * **NoSQL (Non-relational):** Often flexible or "schemaless," using a **schema-on-read** where the application defines the structure *as* data is written.
