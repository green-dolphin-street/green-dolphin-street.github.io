---
title: "Development, Staging, and Pre-production Phases"
layout: single
date: 2025-11-08
categories:
  - IT Infrastructure Engineering
tags:
  - Operation
use_math: true
---

- Development (dev), staging (stg), and pre-production (ppd) are isolated 'environments' for the different phases of development, testing, and release.
- An 'environment' is a self-contained, isolated system where a software application is run and tested. The core purpose is isolation—actions taken in one environment (like a developer's experimental code in dev) cannot break another environment (like the live application in prod).

# ​Dev (Development)
- ​Purpose: This is the primary environment where developers actively write, build, and test their code. It's a "sandbox" for creating new features and fixing bugs.
- ​Key Feature: The environment is fluid and changes constantly. It is not expected to be stable.
- ​Typical Users: Developers.

# ​Stg (Staging)
- Purpose: To provide a testing environment that mirrors the "live" production environment as closely as possible. It is used for final validation before a release.
- Key Feature: Staging is used to catch bugs related to integration, configuration, and data before they affect real users.
- Typical Users: Quality Assurance (QA) engineers, developers doing final checks, and product managers reviewing new features.

# ​PPD (Pre-Production)
- Purpose: This environment is often used interchangeably with "Staging," but in some organizations, it serves as a distinct final check.
- Typical Use: It can be the environment where the exact code and configuration that is "to-be-released" is deployed for a final sign-off or for running automated performance and regression tests.
- Note: If a company has both Stg and PPD, Stg might be for ongoing QA, while PPD is for the "release candidate" (the specific version about to go live).

# How the separation is actually implemented
- The separation of dev, stg, and ppd is often achieved using separate infrastructure, which could mean:
  - ​Different physical servers
  - ​Separate Virtual Machines (VMs)
  - ​Isolated containers (like Docker)
  - ​Separate cloud resource groups or accounts (e.g., in AWS, GCP, or Azure)
- ​Each environment typically has its own version of the code, its own configuration, and its own database.