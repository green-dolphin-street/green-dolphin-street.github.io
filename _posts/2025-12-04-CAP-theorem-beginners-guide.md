---
title: "The CAP Theorem"
layout: single
date: 2025-12-04
categories:
  - Distributed Systems
tags:
  - CAP Theorem
use_math: true
---

## What is the CAP Theorem?
The CAP theorem is a fundamental concept in distributed systems, describing the trade-offs between three key properties:

- **Consistency (C):** Every read receives the most recent write or an error. All nodes see the same data at the same time.
- **Availability (A):** Every request receives a (non-error) response, without guarantee that it contains the most recent write.
- **Partition Tolerance (P):** The system continues to operate despite arbitrary network partitioning (communication breaks between nodes).

## Why Does CAP Matter?
In distributed systems, it's impossible to guarantee all three properties at the same time. You must choose which two to prioritize:

- **CA (Consistency + Availability):** Only possible if the network is reliable (no partitions). Rare in practice.
- **CP (Consistency + Partition Tolerance):** Ensures correct data but sacrifices availability during network splits.
- **AP (Availability + Partition Tolerance):** Ensures high availability but may serve stale data during splits.

## Real-World Examples
- **CP Systems:** HBase, MongoDB (with strong consistency settings)
- **AP Systems:** Couchbase, DynamoDB
- **CA Systems:** Traditional relational databases (when not distributed)

## Key Takeaways
- Network partitions are inevitable in distributed systems.
- You must choose between consistency and availability when a partition occurs.
- Understanding CAP helps you design systems that meet your application's needs.

## Further Reading
- [CAP theorem on Wikipedia](https://en.wikipedia.org/wiki/CAP_theorem)
- [Distributed Systems Principles](https://www.cs.cornell.edu/projects/ladis2008/papers/lakshman-ladis2008.pdf)
