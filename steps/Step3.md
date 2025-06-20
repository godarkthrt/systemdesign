# 🧩 Step 3: Scaling the Backend

> Once your MVP is working, traffic starts to grow. Now you must scale the backend to maintain performance, reliability, and availability.

---

## 🔹 1. Key Concepts

| Concept                | Description                                                 |
| ---------------------- | ----------------------------------------------------------- |
| Load Balancing         | Evenly distributes incoming traffic across multiple servers |
| Caching (App & CDN)    | Reduces redundant DB calls and improves latency             |
| Database Read Replicas | Offloads read traffic from the primary DB                   |
| Scaling Strategies     | Vertical vs Horizontal, proactive vs reactive               |
| Bottlenecks            | CPU, memory, I/O, network — and how to detect them          |

---

## 🔹 2. Detailed Breakdown

---

### 📦 Load Balancing

**What it does:** Distributes requests across multiple backend servers to prevent overloading.

**Common Algorithms:**

- **Round Robin** – Sequentially rotates requests
- **Least Connections** – Sends to the server with fewest active connections
- **IP Hash** – Sticky sessions (e.g., for games, chat)

**Tools:**

- Nginx
- HAProxy
- AWS ELB / GCP Load Balancer

#### 🔧 Load Balancer Features:

- Health checks (remove bad instances)
- SSL termination (offload HTTPS decryption)
- Sticky sessions (if needed)

📈 **Horizontal scaling** of app servers becomes possible now.

---

### ⚡ Caching

**Goal:** Reduce redundant DB calls and serve frequently accessed data faster.

#### 1. **Application-level Cache (Redis / Memcached)**

- Stores key-value data (e.g., user profile, feed)
- Use TTLs to auto-expire cache
- Handle **cache stampede** using locking or stale reads

#### 2. **CDN (Content Delivery Network)**

- Serves static files: images, videos, JS, CSS
- Cached closer to user (edge servers)

📌 Common Providers: Cloudflare, Akamai, AWS CloudFront

#### 🔁 Cache Patterns:

| Pattern       | Use Case                          |
| ------------- | --------------------------------- |
| Cache-Aside   | App checks cache first, then DB   |
| Write-Through | Write to cache and DB together    |
| Read-Through  | Cache handles DB reads internally |

---

### 🗃️ Database Read Replicas

**Why:** Offload read traffic from primary DB, improve read scalability.

**How it works:**

- Primary handles writes
- Replicas sync asynchronously and handle reads

**Tradeoffs:**

- **Stale reads** possible due to replication lag
  - to overcome this systems might use below approaches -
    - Use primary db for sensitive reads e.g. balance related reads/writes go to primary db and less critical reads go to replicas.
    - quorum-based reads (in distributed database like cassandra) which increases consistency guarantees.
    - read-after-write routing -- also called as "read-your-write" consistency or session consistency. [read more](https://arpitbhayani.me/blogs/read-your-write-consistency/)
- Read queries must be idempotent - basically reads must not depend on real-time consistency i.e. they may not reflect immediately after write on primary db. So the read logic should be tolerant of replication lag.

📌 Tools: PostgreSQL, MySQL Replication, Amazon RDS

---

### 🚀 Scaling Strategies

| Type       | Description                           | Example                         |
| ---------- | ------------------------------------- | ------------------------------- |
| Vertical   | Add more CPU, RAM, disk to one server | 64-core DB server               |
| Horizontal | Add more instances (scale-out)        | More app servers, read replicas |
| Proactive  | Scale before failure                  | Based on traffic forecast       |
| Reactive   | Scale after metrics exceed thresholds | Auto-scaling on 80% CPU         |

---

### ⚠️ Common Bottlenecks

| Layer         | Bottleneck Type      | Monitoring Tool / Fix                    |
| ------------- | -------------------- | ---------------------------------------- |
| App Layer     | CPU, thread pool     | APM (Datadog, New Relic), code profiling |
| DB Layer      | Slow queries         | EXPLAIN plans, indexing, read replicas   |
| Network Layer | Latency, packet drop | Ping, traceroute, CDN usage              |
| Cache Layer   | Miss rate, eviction  | Redis MONITOR, eviction policy tuning    |

---

## 🧠 Design Tradeoffs

| Decision             | Pros                         | Cons                              |
| -------------------- | ---------------------------- | --------------------------------- |
| Adding Read Replicas | Scales reads, fast setup     | Replication lag, read consistency |
| Using Cache          | Super fast reads, offload DB | Invalidation issues, stale data   |
| CDN Usage            | Low latency, cost-effective  | Not suitable for dynamic content  |

---

## 🎯 FAANG Mock Interview Questions

### ❓ Question 1:

**"Your backend system is getting 10x more read traffic than write traffic. How would you scale it?"**

✅ Sample Talking Points:

- Add read replicas to offload DB
- Use Redis for hot data caching (e.g., user profiles)
- CDN for static assets
- API gateway with load balancing across app servers

---

### ❓ Question 2:

**"How does a cache-aside strategy work? What are the pitfalls?"**

✅ Sample Answer:

> “App checks Redis first. On cache miss, it fetches from DB and updates cache. Pitfalls include cache stampede during high miss rates and consistency issues on DB update.”

---

### ❓ Question 3:

**"What’s replication lag and why does it matter?"**

✅ Sample Answer:

> “It’s the delay between a write to the primary DB and when that write appears on the replica. It matters because a user may read stale data from a replica right after updating it.”

---

## 💡 Real-World Examples

| Company   | Scaling Tactic Used                                |
| --------- | -------------------------------------------------- |
| Instagram | Uses Redis + Memcached + CDN for fast feed loading |
| YouTube   | Caches metadata and thumbnails at edge             |
| Twitter   | Aggressively caches timelines, uses read replicas  |
| Airbnb    | Employs DB partitioning + Redis for search results |

---

## ✅ Summary Checklist

| Component        | Best Practice                          |
| ---------------- | -------------------------------------- |
| Load Balancer    | Health checks, distribute load evenly  |
| Caching (App)    | Use Redis/Memcached with TTLs          |
| CDN              | Static files (images, JS, videos)      |
| Read Replicas    | Scale reads, monitor lag               |
| Scaling Decision | Scale horizontally if app is stateless |
| Monitoring       | Use metrics/logs to find bottlenecks   |

---
