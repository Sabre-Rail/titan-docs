# ERP Integration Service Modernisation Plan

## Overview

The ERP Integration Service is a .NET Framework 4.8 Windows Service running on the ERP server. It interfaces with a vendor SDK and ERP system that require persistent login. To improve maintainability and deployment reliability, this document outlines a strategy for modernising the service with automated update management, CI/CD integration, fault tolerance, and support for controlled upgrades.

---

## Goals

- Enable automated updates and CI/CD integration for the legacy Windows service
- Reduce pressure and risk during manual upgrades
- Introduce fault tolerance for job processing
- Prepare for zero-downtime or rolling upgrades where feasible

---

## 1. Update Management with Local Agent

### Concept

Instead of deploying updates via remote scripts or DevOps push actions, a **local update agent** service will run on the ERP server. It monitors for new releases and handles updates automatically when conditions are safe (e.g., no active jobs in the queue).

### Agent Responsibilities

- Poll a shared location (Azure Blob, file share, or internal API) for new versions
- Compare version metadata with local version
- Monitor RabbitMQ queues to determine a safe update window
- Perform the update:
  - Stop the ERP Integration Service
  - Backup current binary
  - Replace with new version
  - Restart the service
- Log update activity for auditing and visibility

### Benefits

- Enables CI/CD-driven build and publish without direct server access
- Reduces downtime by ensuring updates only happen during safe periods
- Supports structured and predictable upgrade cycles
- Local logging and optional status API for visibility

---

## 2. CI/CD Integration (Build & Publish Only)

- Azure DevOps Pipeline builds the Windows Service and packages it as `.zip` or `.msi`
- The artifact is pushed to a monitored location:
  - Azure Blob Storage
  - Internal file share
  - HTTP endpoint for version metadata

> **Note**: The deployment *execution* is handled by the update agent, not by DevOps directly.

---

## 3. Fault Tolerance Architecture

### Queue-Based Processing

- All long-running operations are handled asynchronously via RabbitMQ
- Ensures user-facing systems are decoupled from ERP system delays
- Supports retry logic and dead-letter handling for fault isolation

### Redundant Service Design

- If ERP/vendor SDK allows multiple instances:
  - Use multiple service consumers with queue deduplication and idempotency
- If limited to a single active instance:
  - Implement leader election or locking to prevent concurrency
  - Fallback instance remains dormant unless the active instance fails

---

## 4. Upgrade and Resilience Strategy

### Safe Upgrades via Update Agent

- New versions are staged and only applied once the queue is drained
- Ensures ongoing tasks are not disrupted
- Allows blue-green or side-by-side strategies to be considered in the future

---

## 5. Next Steps

### Immediate Tasks

- [ ] Develop the Update Agent as a .NET Worker Service
- [ ] Add version and health endpoints to ERP Integration Service
- [ ] Create Azure DevOps pipeline for building and publishing the service

### Medium-Term Goals

- [ ] Implement fault-tolerant job handling with retries and dead-letter queues
- [ ] Evaluate multiple-instance service support
- [ ] Add visibility via logs or lightweight web dashboard

### Long-Term Vision

- [ ] Replace legacy SDK with .NET-compatible abstraction
- [ ] Fully containerise if/when vendor SDK is modernised
