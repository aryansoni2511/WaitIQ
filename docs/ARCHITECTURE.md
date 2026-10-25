# WaitIQ Architecture Specification

> **Notice:** This document defines the proposed system architecture for WaitIQ. It represents design specifications and architectural guidelines; it is not an implemented or tested system. No application source code has been implemented in this phase.

---

## 1. Architecture Style: Modular Monolith

WaitIQ is structured as a **Modular Monolith**.

### Why Modular Monolith?
- **Operational Simplicity:** A single deployable backend eliminates distributed system complexities (service discovery, distributed transactions, network latency between microservices).
- **Clear Boundaries:** Modules (Authentication, Token Management, Queue Service, Notification Engine) are partitioned logically within distinct layers.
- **Maintainability:** Ideal for an MVP serving initial targets (clinics and salons) while preserving straightforward future extraction into standalone services if horizontal scale demands it.

---

## 2. Component Responsibilities

### 2.1 Frontend Responsibilities (React + Vite)
- **Customer Interface:**
  - Lightweight, mobile-responsive page accessible via QR code / link.
  - Displays current token number, live queue position, and estimated wait time.
  - Requires no customer registration or application download.
- **Receptionist & Business Operator Dashboard:**
  - Interface for managing daily queues: calling next token, skipping, marking in-progress, or completing service.
  - Walk-in token issuance interface.
- **API Client & State Management:**
  - Communicates with backend endpoints using standard HTTP requests.
  - Manages polling intervals for queue position refreshes.

### 2.2 Backend Responsibilities (Node.js + Express)
- **Token Lifecycle Management:** Creation, validation, status updates, and cancellation.
- **Queue Orchestration:** Ordering tokens, assigning sequence numbers, managing daily queue resets.
- **Wait-Time Estimation Engine:** Calculating dynamic ETAs based on average service duration and remaining waiting tokens.
- **Authentication & Authorization:** Secure session/token handling for business staff and managers.
- **Notification Integration (Future Phase):** Dispatching turn-approaching alerts via SMS/WhatsApp/Push.

### 2.3 Database Responsibilities (PostgreSQL)
- **Businesses & Branches:** Multi-tenant support storing business profiles, working hours, and operational counters.
- **Queue Configurations:** Service categories (e.g., Doctor Consultation, Haircut, Facial) with default service duration baselines.
- **Tokens & State Ledger:** Comprehensive audit log of every token generated, assigned sequential order, and state changes.
- **Timestamps & Analytics:** Recording issuance time, service start time, and completion time to refine machine-learned or statistical wait-time estimation.

---

## 3. Token State Machine & Lifecycle

Tokens transition through strict states:

```
[ WAITING ] ───────► [ IN_PROGRESS ] ───────► [ COMPLETED ]
     │                      │
     ▼                      ▼
[ CANCELLED ]         [ CANCELLED ]
```

### Initial Token States:
1. `WAITING`: Token generated and in line; customer has not been called yet.
2. `IN_PROGRESS`: Customer's turn has arrived; service or consultation is currently being provided.
3. `COMPLETED`: Service finished; token lifecycle successfully finalized.
4. `CANCELLED`: Customer withdrew, was a no-show, or operator cancelled the token.

---

## 4. Queue Consistency & Concurrency Control

- **Server-Authoritative State:** Client-side state must never dictate or assume queue transitions. The server and database remain the single source of truth.
- **Database Transactions:** Operations altering token states (e.g., advancing a queue, calling the next token) must execute inside database transactions (`BEGIN...COMMIT`) with row-level locks where necessary to prevent race conditions when multiple operators work simultaneously.
- **Unique Constraints:** Prevent duplicate active token sequences per queue per business day.

---

## 5. Live Updates Strategy

- **Phase 1 Implementation: Periodic Polling:**
  - Clients poll the queue status endpoint at configurable intervals (e.g., every 15–30 seconds when active, backing off if inactive).
  - Simple, robust over variable mobile networks, and requires minimal infrastructure overhead.
- **Future Evaluation: WebSockets:**
  - Full-duplex connections (via Socket.io or native WebSockets) will be evaluated when concurrent real-time demand and immediate push requirements justify persistent server socket management.

---

## 6. Security & Privacy Principles

- **Credential & Secret Protection:** All database URLs, API secrets, and JWT signing keys reside in `.env` files and must never be committed to version control.
- **Input Validation & Sanitization:** Strict schema validation on all incoming API payloads to prevent injection attacks and invalid data states.
- **Role-Based Access Control (RBAC):** Public customer views are read-only and restricted to the specific token identifier. Operator actions require authenticated credentials.
- **Privacy by Design:** Avoid collecting sensitive personal health or private data. Only operational data (e.g., display name, token number, phone number for notification alerts) is collected.

---

## 7. Future Deployment Considerations

- Containerization via Docker for deterministic local and production environments.
- Managed PostgreSQL instance with automated backups and connection pooling (e.g., PgBouncer).
- CDN hosting for Vite-built static frontend assets.

---

## 8. Current Implementation Status

- **Status:** Architectural specification only.
- **Unimplemented Components:** Backend APIs, database connections, frontend components, and automated test suites have not yet been implemented.
