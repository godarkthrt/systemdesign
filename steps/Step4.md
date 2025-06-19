# 🧩 Step 4: Scaling Storage (Sharding, NoSQL, CAP Theorem)

> As your system grows, a single database becomes a bottleneck. You need to scale **storage, consistency, and performance**.

---

## 🔹 1. Key Concepts

| Term               | Description                                                         |
| ------------------ | ------------------------------------------------------------------- |
| Sharding           | Partitioning data horizontally across DBs/nodes                     |
| NoSQL              | Non-relational databases for scalability and flexibility            |
| CAP Theorem        | Tradeoff between Consistency, Availability, and Partition Tolerance |
| Consistency Models | Defines how up-to-date or synchronized data is across nodes         |

---

## 🔹 2. Detailed Breakdown

---

### 🍰 What is Sharding?

**Sharding** (horizontal partitioning) means **splitting your data across multiple databases** (or DB instances) based on some key.

#### ✅ Benefits

- Enables **horizontal scaling**
- Prevents one DB from becoming a bottleneck
- Can isolate "hot" data

#### 🧠 How It Works:

```
UserID % 3
→ Shard 0 → DB0
→ Shard 1 → DB1
→ Shard 2 → DB2
```

So:

- UserID 120 → Shard 0
- UserID 121 → Shard 1
- UserID 122 → Shard 2

---

#### 🛠 Shard Key Design

| Key Type         | Example            | When to Use                      |
| ---------------- | ------------------ | -------------------------------- |
| Hash-based       | `hash(user_id)`    | Even distribution of traffic     |
| Range-based      | `timestamp`, `ID`  | Time-series, ordered data        |
| Geo/Entity-based | `region`, `org_id` | Isolate tenants, better locality |

---

#### ⚠️ Sharding Tradeoffs

- **Complex joins** across shards are difficult
- **Rebalancing** shards is hard (e.g., when adding more shards)
- Need **routing logic** (at app or middleware level)

---

### 📚 NoSQL – Scale Beyond Relational

**When to use:** When you need to scale writes, reduce schema rigidity, or handle semi-structured/unstructured data.

#### 🔹 Types of NoSQL

| Type              | Example DBs      | Use Case                                |
| ----------------- | ---------------- | --------------------------------------- |
| Key-Value         | Redis, DynamoDB  | Caching, session store                  |
| Document Store    | MongoDB, CouchDB | Semi-structured JSON data               |
| Wide Column Store | Cassandra, HBase | High write throughput, log storage      |
| Graph DB          | Neo4j            | Relationship-heavy data (social graphs) |

---

#### ⚖️ NoSQL vs SQL Tradeoffs

| Aspect       | SQL (RDBMS)           | NoSQL                       |
| ------------ | --------------------- | --------------------------- |
| Schema       | Fixed schema          | Flexible schema             |
| Joins        | Strong support        | Usually not supported       |
| Transactions | Full ACID             | Usually eventual or BASE    |
| Scaling      | Vertical (by default) | Horizontal (by design)      |
| Use Case     | Banking, ERP          | Social apps, IoT, analytics |

---

### 🧩 CAP Theorem (Consistency, Availability, Partition Tolerance)

> You can only choose **2 out of 3** in a distributed system during network partitions.

#### 🔺 Triangle Summary

```
            Consistency
              /     \
             /       \
            /         \
Availability --------- Partition Tolerance
```

#### CAP Definitions:

- **Consistency** – All clients see the same data at the same time
- **Availability** – Every request receives a (non-error) response
- **Partition Tolerance** – System works even if network is split

#### 💡 Examples:

| System          | CAP Category         | Notes                                          |
| --------------- | -------------------- | ---------------------------------------------- |
| MongoDB         | CP                   | Strong consistency, can sacrifice availability |
| Cassandra       | AP                   | High availability, allows stale data           |
| DynamoDB        | Tunable              | You can choose consistency per read/write call |
| Traditional SQL | CA (non-distributed) | No partition tolerance                         |

---

### 📜 Consistency Models

| Model                | Description                        | Use Case                |
| -------------------- | ---------------------------------- | ----------------------- |
| Strong Consistency   | Always returns latest data         | Banking, transactions   |
| Eventual Consistency | Updates propagate over time        | Social feeds, analytics |
| Causal Consistency   | Preserves causality between events | Chat, messaging         |
| Read-Your-Writes     | User sees their own updates        | Profile changes         |

---

## 🧠 Design Tradeoffs

| Decision           | Pros                                       | Cons                                   |
| ------------------ | ------------------------------------------ | -------------------------------------- |
| Sharding           | Scales horizontally, avoids DB bottlenecks | Complex joins, rebalancing is hard     |
| NoSQL              | Fast writes, flexible schema               | Weak consistency, poor transactions    |
| SQL                | Strong schema + ACID                       | Harder to scale horizontally           |
| Strong Consistency | Predictable results                        | Higher latency or reduced availability |

---

## 🎯 FAANG Mock Interview Questions

### ❓ Question 1:

**"Design a high-throughput analytics system that ingests logs at scale. What DB would you use?"**

✅ Sample Answer:

> “I’d use a wide-column store like Cassandra or Bigtable because they’re optimized for heavy writes and time-series data. We can shard logs by date or source, and use eventual consistency to maintain throughput.”

---

### ❓ Question 2:

**"What is a good shard key, and what happens if it's poorly chosen?"**

✅ Talking Points:

- A good shard key evenly distributes data and queries.
- Poor keys (e.g., timestamp) can cause **hot partitions**.
- App must route requests correctly based on the shard key.

---

### ❓ Question 3:

**"Your database is maxed out. How would you shard it?"**

✅ Sample Approach:

- Analyze the data access pattern
- Choose a shard key (e.g., `user_id`)
- Add a routing layer or middleware
- Move data in batches (live migration strategy)
  - #TODO read more about how to achieve this

---

### ❓ Question 4:

**"Explain the CAP theorem with an example where you would choose availability over consistency."**

✅ Sample Answer:

> “For a social media newsfeed, I can tolerate slightly stale data to keep the system always available. Using eventual consistency via a NoSQL DB (like DynamoDB or Cassandra) ensures users always see something, even if it’s not up to the last second.”

---

## 💡 Real-World Systems

| Company  | Storage Strategy                                     |
| -------- | ---------------------------------------------------- |
| Facebook | Shards user data by user ID + NoSQL for feed caching |
| LinkedIn | Uses Espresso (custom distributed DB) for messaging  |
| Netflix  | Uses Cassandra for write-heavy workloads             |
| Amazon   | DynamoDB for availability, plus S3 for archival      |

---

## ✅ Summary Checklist

| Topic              | Key Point                                        |
| ------------------ | ------------------------------------------------ |
| Sharding           | Horizontal scaling via partitioning              |
| NoSQL              | Flexible, scalable, often eventually consistent  |
| CAP Theorem        | Choose 2 of Consistency, Availability, Partition |
| Read-Your-Writes   | Useful for showing user's own updates            |
| Consistency Models | Choose based on use case                         |
| Shard Key          | Must distribute both data and traffic evenly     |

---

## ➡️ Next Step: Step 5 – Data Synchronization and Asynchronous Processing

We’ll cover:

- Message queues (Kafka, RabbitMQ)
- Eventual consistency via events
- Async workflows (notifications, emails, etc.)
- Pub/Sub vs Event Sourcing
