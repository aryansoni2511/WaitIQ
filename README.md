# WaitIQ

WaitIQ is an intelligent queue and waiting-time management platform designed to optimize queue flow, eliminate physical waiting room congestion, and provide transparency for both customers and service providers.

---

## Problem Statement

In places like clinics and salons, customers and patients often spend hours waiting physically without clear visibility into when their turn will arrive. This creates crowded waiting areas, increases anxiety, and leads to operational inefficiencies for staff. WaitIQ bridges this gap by providing real-time digital queue tracking and estimated wait times.

---

## Target Audience (Initial Focus)

- **Clinics & Healthcare Centers:** Managing patient consultation queues and doctor availability delays.
- **Salons & Wellness Spas:** Managing stylist availability and variable service duration queues.
- *Future Extensions:* Diagnostics labs, customer service desks, and food counters.

---

## Core Workflow

### Customer Journey
1. **Token Generation:** The customer receives a digital token by scanning a QR code at the business entrance or via the receptionist.
2. **Live Queue Tracking:** The customer views their live queue position and estimated waiting time on their mobile browser without installing an application.
3. **Turn Nearing Alert:** When their turn approaches, an alert notification is sent to the customer (once notification services are implemented).
4. **Service Delivery:** The customer arrives at the premises right on time and receives service without unnecessary waiting.

### Receptionist / Operator Workflow
- Monitor current queue status and token lifecycle.
- Call the next customer, advance tokens, or mark status as in-progress or completed.
- Handle walk-in requests and cancellations.

---

## Planned Technology Stack

- **Frontend:** React + Vite (Fast, modular, client-side interface)
- **Backend:** Node.js + Express (RESTful APIs and queue state orchestration)
- **Database:** PostgreSQL (Relational consistency, transactional safety, and robust indexing)
- **Live Updates Strategy:** Periodic client polling for the initial phase, with evaluation of WebSockets (Socket.io) as load and usage warrant.

---

## Development Roadmap & Planned Phases

1. **Phase 1: Architecture & Planning** *(Completed)*
2. **Phase 2: Project & Documentation Foundation** *(Current)*
3. **Phase 3: Backend & Database Schema Definition** *(Planned)*
4. **Phase 4: Core Queue & Token Management APIs** *(Planned)*
5. **Phase 5: Frontend Customer & Operator Interfaces** *(Planned)*
6. **Phase 6: Wait-Time Estimation Algorithms** *(Planned)*
7. **Phase 7: End-to-End Testing & Verification** *(Planned)*
8. **Phase 8: Production Deployment & Monitoring** *(Planned)*

---

## Project Status

> **Current Status: Phase 2 — Foundation Stage**<br />
> Application source code, dependencies, and database connections have **not** been implemented yet. This repository currently contains architectural specifications, documentation, and Git version control configuration.

---

## Repository

- **GitHub URL:** [https://github.com/aryansoni2511/WaitIQ.git](https://github.com/aryansoni2511/WaitIQ.git)

---

## Setup Instructions

Detailed setup and installation instructions will be added as application packages and runtime environments are implemented in subsequent phases.
