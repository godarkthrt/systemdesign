# 🧩 Step 5: Async Processing and Data Flow

> As systems grow, many tasks no longer need to be done synchronously. Async processing helps improve performance, scalability, and reliability.

---

## 🔹 1. Key Concepts

| Concept              | Description                                              |
| -------------------- | -------------------------------------------------------- |
| Message Queue        | Decouple services by using queues to store async tasks   |
| Eventual Consistency | Data syncs over time across systems (acceptable delay)   |
| Background Jobs      | Tasks handled outside of request-response lifecycle      |
| Pub/Sub              | Publisher sends events, multiple subscribers handle them |
| Event Sourcing       | Store state changes as a sequence of events              |

---

## 🔹 2. Detailed Breakdown

---

### 📬 Message Queues

**Why?**  
Async tasks shouldn’t block the main user request. Example: sending an email after signup.

#### 🧰 Tools

- Kafka (durable, distributed, high-throughput)
- RabbitMQ (lightweight, reliable delivery)
- Amazon SQS / Google Pub/Sub (fully managed)

#### 🔁 Typical Flow:

```
User Request → API Server → Push to Queue → Worker → DB / Email / Notification
```

### 🔄 Producer / Consumer Model

| Role     | Description                        |
| -------- | ---------------------------------- |
| Producer | Sends task to a message queue      |
| Consumer | Listens to the queue, processes it |

---

### ⌛ Background Job Use Cases

- Send welcome email
- Process video/audio
- Generate reports
- Push mobile notifications
- Clean up temporary files
- Sync data between systems

---

### 📬 Pub/Sub Architecture

> **One-to-many** pattern where **producers (publishers)** emit events that are received by **multiple consumers (subscribers)**.

#### Diagram:

```
+----------+          +--------------------+
| Producer | ───────▶ | Topic/Event Stream |
+----------+          +--------------------+
                           ▲        ▲
                           │        │
                   +--------------+ +--------------+
                   | Subscriber A | | Subscriber B |
                   +--------------+ +--------------+

```

🧠 This allows:

- Loose coupling between services
- Independent scaling of consumers
- Flexible workflows (e.g., analytics + email from same event)

---

### ⚙️ Eventual Consistency

In async systems, different components might not be updated **instantly**, but they **will converge** over time.

#### Example:

1. Order placed → API writes to DB
2. Sends message: "Order Created"
3. Inventory service consumes event, updates stock
4. Shipping service queues package

⏱️ These steps may happen seconds apart, but **system remains eventually consistent**.

---

### 📜 Event Sourcing

Instead of storing current state, store **every state change as an event**:

| Event # | Type         | Data                         |
| ------- | ------------ | ---------------------------- |
| 1       | UserCreated  | { id: 123, name: "Sam" }     |
| 2       | EmailUpdated | { id: 123, email: "a@b.com"} |
| 3       | UserDeleted  | { id: 123 }                  |

✅ Replaying events = reconstructing current state  
✅ Useful for audit trails and debugging

---

### 🧠 Design Tradeoffs

| Decision             | Pros                                       | Cons                                  |
| -------------------- | ------------------------------------------ | ------------------------------------- |
| Async processing     | Improves responsiveness, decouples systems | Harder to debug, adds complexity      |
| Queues               | Retry, durability, decoupling              | Can become bottleneck or single point |
| Pub/Sub              | Flexible fan-out processing                | Message ordering can be tricky        |
| Eventual consistency | Highly scalable                            | Requires careful client-side design   |

---

## 🎯 FAANG Mock Interview Questions

---

### ❓ Question 1:

**"Design an email delivery system. Should the email be sent in the same request that creates the user?"**

✅ Talking Points:

- No: Email sending should be async
- Use a message queue (e.g., Kafka or RabbitMQ)
- Worker listens to queue and triggers SMTP/email service

---

### ❓ Question 2:

**"What happens if a consumer fails while processing a queue message?"**

✅ Answer:

> Depends on queue configuration. In Kafka, the message won’t be committed and can be reprocessed. In RabbitMQ, messages can be re-queued if consumer fails.

---

### ❓ Question 3:

**"Explain how you'd keep multiple microservices eventually consistent without using a monolith."**

✅ Sample Answer:

> Use an event-driven approach. Services publish events like "UserCreated" or "OrderShipped", and others subscribe and react accordingly. Use idempotent consumers and dead-letter queues for reliability.

---

## 💡 Real-World Systems

| System    | Async Tech Used                               |
| --------- | --------------------------------------------- |
| Uber      | Kafka for ride events, surge pricing          |
| Netflix   | Kafka for log aggregation and recommendations |
| Facebook  | Event logs, job queues, real-time analytics   |
| Instagram | Celery (Python) for feed generation           |

---

## ✅ Summary Checklist

| Topic                | Best Practice                               |
| -------------------- | ------------------------------------------- |
| Message Queue        | Use for decoupling and retryable tasks      |
| Pub/Sub              | When multiple services need the same event  |
| Background Jobs      | For emails, notifications, media processing |
| Eventual Consistency | Accept delay but ensure convergence         |
| Idempotent Consumers | Retry-safe and side-effect-free consumers   |
| Monitoring           | Use DLQs, metrics, and trace logs           |

---
