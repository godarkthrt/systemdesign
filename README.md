# 🧱 System Design Roadmap (From Scratch to Scale)

---

## 🎯 Goals

- Crack FAANG-level system design interviews
- Master backend architecture to build scalable systems

---

## 🗺️ Evolution Stages

### 🚩 Step 1: Requirements Gathering

**🎯 Purpose:** Clarify what to build and how it will be used

#### 🔹 Functional Requirements

- Core features the system should support
- Example: User can upload photos, comment, like

#### 🔹 Non-Functional Requirements

- Performance, scalability, availability, latency
- Example: Handle 10M users, <200ms latency, 99.99% uptime

#### 🧠 Interview Notes

- Ask clarifying questions:
  - Read-heavy or write-heavy?
  - Consistency vs availability?
  - Expected traffic? (RPS, DAU, concurrent users)
  - Data growth over time?
  - Uptime SLAs?

---

### ⚙️ Step 2: MVP Architecture

**🎯 Purpose:** Build a simple, maintainable, working system

#### 🔹 Basic Components

- Frontend (web/mobile)
- API Gateway / Reverse Proxy (e.g., Nginx, etc.. [learn more](./apigatewayVsReverseproxy.md) )
- Application Server (e.g., Flask, Spring Boot)
- Relational Database (e.g., PostgreSQL, MySQL)

#### 🧠 Interview Notes

- Prefer monolith early on
- Use REST APIs, keep backend stateless
- Talk about clean API design, MVC separation

---

### 📈 Step 3: Scaling Backend (Users Growing)

**🎯 Purpose:** Improve performance and availability

#### 🔹 Key Concepts

| Concept              | Use Case                                      |
| -------------------- | --------------------------------------------- |
| Load Balancing       | Distribute requests (Nginx, AWS ELB, HAProxy) |
| Caching (Redis, CDN) | Speed up reads, reduce DB load                |
| Indexing             | Improve DB read performance                   |
| Horizontal Scaling   | Add more app/DB servers                       |

#### 🧠 Interview Notes

- Cache: Redis (hot data), CDN (static files)
- Mention: Cache invalidation, stampede, TTLs
- Scale read-heavy systems with replicas

---

### 💾 Step 4: Scaling Storage (Data Growth)

**🎯 Purpose:** Manage large-scale data and avoid DB bottlenecks

#### 🔹 Key Concepts

| Concept                  | Why Important                             |
| ------------------------ | ----------------------------------------- |
| Sharding (Partitioning)  | Split data across machines                |
| Data Modeling            | Normalize vs denormalize for access speed |
| NoSQL (Mongo, Cassandra) | Schema flexibility, horizontal scaling    |
| CAP Theorem              | Understand tradeoffs in distributed data  |

#### 🧠 Interview Notes

- Choose sharding keys carefully (e.g., user_id)
- Talk about eventual consistency in NoSQL
- Explain why/when to migrate from RDBMS

---

### 🧠 Step 5: Async Processing & Background Jobs

**🎯 Purpose:** Decouple heavy/slow tasks from user experience

#### 🔹 Key Concepts

| Concept                  | Use Case                     |
| ------------------------ | ---------------------------- |
| Message Queue (Kafka)    | Decouple producers/consumers |
| Task Queue (Celery)      | Run background jobs          |
| Retry/Dead-letter Queues | Handle failures gracefully   |

#### 🧠 Interview Notes

- Use case: image/video processing, email, analytics
- Discuss at-least-once vs exactly-once semantics
- Consider idempotency in task processing

---

### 🏰 Step 6: High Availability & Fault Tolerance

**🎯 Purpose:** Keep system up and resilient during failures

#### 🔹 Key Concepts

| Concept | Why Important |
| ------- | ------------- |
