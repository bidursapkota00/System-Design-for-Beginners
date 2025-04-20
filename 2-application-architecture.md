# Application Architecture

## Overview

This lesson covers how applications are structured and deployed across different environments. Understanding application architecture is essential to build scalable and maintainable systems.

## Key Architectures

### 1. Monolithic Architecture

- A single, unified application.
- Easy to develop and deploy initially.
- Difficult to scale or maintain over time as the codebase grows.

### 2. Microservices Architecture

- Breaks the application into smaller, independent services.
- Each service can be deployed and scaled independently.
- Requires robust communication and monitoring infrastructure.

### 3. Client-Server Model

- The server hosts, delivers, and manages resources and services.
- The client (browser, app) sends requests to the server and displays data.
- Common in web and mobile apps.

### 4. N-Tier Architecture

- Layers include presentation, logic, and data.
- Promotes separation of concerns.
- Easier to manage and test each layer independently.

### 5. Service-Oriented Architecture (SOA)

- Similar to microservices but typically uses enterprise service bus (ESB) for communication.
- Focuses on reusability and integration of services.

## Deployment Considerations

- Load balancing, redundancy, failover, and containerization (e.g., Docker).
- Use of orchestrators like Kubernetes for large-scale deployments.

## Summary

Choosing the right application architecture is crucial for:

- Scalability
- Maintainability
- Deployment agility
- Performance under load

---
