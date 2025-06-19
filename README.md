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
- API Gateway / Reverse Proxy (e.g., Nginx)
  - [API Gateway Vs Reverse Proxy](./good%20reads/apigatewayVsReverseproxy.md)
- Application Server (e.g., Spring Boot , Flask)
  - [app server Vs web server](./good%20reads/webserverVsAppserver.md)
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
  - #TODO read more about this in terms of kafka or other places how it is handled
- Consider idempotency in task processing

---

### 🏰 Step 6: High Availability & Fault Tolerance

**🎯 Purpose:** Keep system up and resilient during failures

#### 🔹 Key Concepts

| Concept       | Why Important                        |
| ------------- | ------------------------------------ |
| Replication   | No single point of failure           |
| Failover      | Automatic backup takeovers           |
| Backpressure  | Prevent overload                     |
| Health Checks | Detect and route around failed nodes |

#### 🧠 Interview Notes

- Talk about active-passive DBs, redundancy
- Mention circuit breakers, rate limits
- Design for graceful degradation (limited features during failure)

---

### 🔐 Step 7: Security & Observability

**🎯 Purpose:** Protect and monitor the system in production

#### 🔹 Security Concepts

| Concept        | Use Case                           |
| -------------- | ---------------------------------- |
| Authentication | Verify user identity (JWT, OAuth2) |
| Authorization  | Control access to resources        |
| TLS / HTTPS    | Encrypt data in transit            |
| Rate Limiting  | Prevent abuse/DDoS                 |

#### 🔹 Observability Concepts

| Concept             | Tool/Practice         |
| ------------------- | --------------------- |
| Logging             | ELK stack, Fluentd    |
| Metrics             | Prometheus + Grafana  |
| Distributed Tracing | Jaeger, OpenTelemetry |

#### 🧠 Interview Notes

- Secure APIs: OAuth2, JWT, HTTPS everywhere
- Include alerts, dashboards, logs
- Talk about zero-trust security, audit logs

---

## 🧭 Suggested Study Path

1. ✅ Requirements Gathering + MVP Design
2. 🔄 Load Balancing, Caching, DB Scaling
3. 🧵 Asynchronous Architecture
4. 🧱 Sharding, CAP, NoSQL Patterns
5. 🏰 High Availability & Reliability
6. 🔐 Security + Observability
7. 🔧 Real-world System Design Cases (Instagram, YouTube, WhatsApp)

---

## 🧪 Interview Pro Tips

- Always **clarify requirements first**
- Start with simple design, then evolve
- Use **diagrams**: component, sequence, flow
- Mention tradeoffs at each decision point
- Prepare **real-world case studies** (e.g., Instagram feed, Dropbox file storage)
- Don’t forget: **Monitoring, Testing, Deployment, Security**

---
