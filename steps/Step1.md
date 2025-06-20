# 🧩 Step 1: Requirements Gathering

> First and most critical phase in any system design. It sets the direction for the entire architecture.

---

## 🔹 1. Key Concepts

| Term                        | Definition                                                          |
| --------------------------- | ------------------------------------------------------------------- |
| Functional Requirements     | Features the system must perform (e.g., upload photo, send message) |
| Non-Functional Requirements | System qualities like latency, uptime, scalability                  |
| Constraints                 | External or internal limits (e.g., use MySQL, global access needed) |

---

## 🔹 2. Detailed Breakdown

### 📌 Functional Requirements

**What it is:**

- Directly relates to the **core business features** and **user experience**.

**Examples:**

- Users can register, login, and logout
- Users can upload, delete, and view images
- Users can follow other users
- Real-time notifications when someone likes a post

**Interview Tips:**

- Ask: “What are the core user flows we need to support?”
- Prioritize by importance: MVP first, then scale

---

### 📌 Non-Functional Requirements

**What it is:**

- Defines **how well** the system performs its functions.

**Common Categories:**

| Category        | Examples                                        |
| --------------- | ----------------------------------------------- |
| Performance     | Latency < 200ms for 95% of requests             |
| Scalability     | Handle 1M DAUs, 10K QPS                         |
| Availability    | 99.99% uptime target (downtime ≤ ~52 mins/year) |
| Maintainability | Easy to deploy, rollback, and monitor           |
| Consistency     | Strong, eventual, or causal consistency         |
| Durability      | Data should never be lost, even in server crash |

**Interview Tips:**

- Ask: “What are our SLAs for latency, throughput, and uptime?”
- Clarify traffic patterns: peak load vs average

---

### 📌 Constraints

**What it is:**

- Boundaries around your design. Could be technical, legal, or operational.

**Examples:**

- System must be GDPR compliant (legal)
- Must use PostgreSQL (technical)
- Needs to support users from India, Europe, and US (infra + latency)
- No third-party services allowed (security/cost)

**Interview Tips:**

- Always ask: “Any tech, budget, or compliance constraints?”
- Understand team/stack limitations

---

## 🔹 3. Interview Tactics

### ✅ Questions to Ask (Clarifying)

- What are the core user flows?
- Expected number of:
  - Daily Active Users (DAU)?
  - Concurrent users?
  - Requests per second (RPS)?
- Read-heavy or write-heavy?
- What’s the target response time?
- How much data are we storing monthly/yearly?
- Global or regional audience?
- What happens if a feature fails? (tolerance)
- Must the system support mobile, web, or both?

---

## 🔹 4. Real-World Insights

### Facebook

- Started with simple profile + wall → scaled to global social graph
- Initial system handled campus use cases only (limited scope)
- Requirements evolved rapidly once user base exploded

### WhatsApp

- Started with core focus on message delivery + offline persistence
- Focused on **latency and reliability** over feature richness
- Gradually added group chat, media, and encryption

### Uber

- Initial: Rider requests → Driver assigned → Trip tracking
- Eventually required real-time maps, dynamic pricing, surge logic
- Each feature became a full subsystem

---

## 🔹 5. Summary Checklist

| Category                    | Questions to Ask                           |
| --------------------------- | ------------------------------------------ |
| Functional Requirements     | What are the core user flows or features?  |
| Non-Functional Requirements | What are the performance/scale goals?      |
| Constraints                 | Any tech, legal, geographic limits?        |
| Traffic Patterns            | What’s the expected load (RPS, DAU)?       |
| Failure Tolerance           | What happens if X service fails?           |
| Growth Projections          | How much data/users growth in 6–12 months? |

---

## ✅ Your Goal for This Step

Before designing anything, be 100% clear on:

- What you’re building
- Why you’re building it
- For whom you’re building it
- Under what expectations and limits

---
