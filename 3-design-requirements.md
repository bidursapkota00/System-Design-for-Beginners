# Design Requirements

## Overview

Before designing a system, it's essential to understand what the system is supposed to do. This lesson focuses on identifying and categorizing different types of requirements that guide architecture and design decisions.

## Key Types of Requirements

### 1. Functional Requirements

- Define **what** the system should do.
- Examples:
  - Allow users to register and log in.
  - Process user payments.
  - Display real-time data on a dashboard.

### 2. Non-Functional Requirements (NFRs)

- Define **how well** the system performs its functions.
- Common categories:
  - **Scalability**: Can the system handle growing loads?
  - **Reliability**: Is it fault-tolerant and always available?
  - **Performance**: Latency, response time, throughput.
  - **Security**: Data protection, authentication, authorization.
  - **Maintainability**: Ease of updating and debugging.
  - **Compliance**: Meets legal or organizational policies.

### 3. System Constraints

- Limitations imposed on design and implementation.
- Examples:
  - Budget and hardware limits.
  - Programming language or tech stack.
  - Regulatory constraints (e.g., GDPR).

### 4. Estimating Scale

Understanding system scale is critical:

- Number of users (concurrent and daily active users).
- Read/write operations per second.
- Data storage needs (in MBs, GBs, or TBs).
- Expected growth over time.

## Why It Matters

- Poor understanding of requirements can lead to over-engineering or under-engineering.
- Clear requirements ensure the system meets user needs and performs under real-world conditions.

## Summary

Design requirements form the blueprint of the entire system. Every design decision should trace back to a requirement.

---
