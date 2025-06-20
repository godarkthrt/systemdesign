# 🧩 Step 6: High Availability (HA) & Fault Tolerance (FT)

> Design systems that stay up even when parts fail.  
> Think: **"How can my system survive hardware failure, network issues, or traffic spikes?"**

---

## 🔹 1. Key Concepts

| Concept           | Description                                                            |
| ----------------- | ---------------------------------------------------------------------- |
| High Availability | System remains accessible (uptime) even when failures occur            |
| Fault Tolerance   | System continues working correctly even if a component fails           |
| Redundancy        | Extra components to replace failures (active-passive or active-active) |
| Failover          | Automatic switch to a backup instance/node                             |
| Load Balancing    | Distribute traffic across healthy instances                            |
| Health Checks     | Probes to detect failure and remove bad nodes from rotation            |

---

## 📶 2. Availability Targets

| SLA Target        | Downtime per month |
| ----------------- | ------------------ |
| 99.9% (3 nines)   | ~43 minutes        |
| 99.99% (4 nines)  | ~4.3 minutes       |
| 99.999% (5 nines) | ~26 seconds        |

🔹 FAANG targets often start at 4–5 nines for critical services.

---

## 🧠 3. Techniques for High Availability

---

### 🔁 Load Balancing

Distributes traffic across multiple instances.

| Type        | Description                  |
| ----------- | ---------------------------- |
| Round Robin | Equal distribution           |
| Least Conn  | Send to least busy instance  |
| Geo-based   | Route based on user location |

✅ Use with health checks to avoid routing to bad nodes.

---

### 🗃️ Redundancy & Replication

- **DB Replication** (read replicas, failover secondaries)
- **Multi-AZ / Multi-region deployment** for resilience
- **Stateless services** can be restarted or scaled easily

---

### 🔄 Auto Healing & Failover

| Component        | Technique                            |
| ---------------- | ------------------------------------ |
| VM / Pod Failure | Auto-restart via Kubernetes / ECS    |
| DB Failure       | Promote secondary (RDS, MySQL group) |
| Service Failure  | Reroute traffic, restart container   |

✅ Use **orchestration tools** (like Kubernetes) to self-heal and reschedule services.

---

### 🔌 Circuit Breakers

Prevent cascading failures:

- **Trip** when error threshold is exceeded
- Temporarily **block calls** to the failing component
- **Retry** after cool-down

🛠️ Libraries: Netflix Hystrix (deprecated), Resilience4j

---

### 🔥 Graceful Degradation

System reduces functionality without total failure.

| Scenario             | Fallback                  |
| -------------------- | ------------------------- |
| Search service down  | Return cached results     |
| Recommendations fail | Show popular items        |
| Payment gateway down | Save cart for retry later |

---

### 🌍 Multi-Region Deployments

For ultra-high availability:

- **Active-Active**: All regions serve live traffic
- **Active-Passive**: One region on standby
- **Latency-based routing** via DNS (e.g., AWS Route53)

📌 Handle **data consistency** and **replication lag** in this model.

---

## 🧪 4. Monitoring & Detection (Support Layer)

We already covered this as a full section, but recap here for context:

| Layer    | Example Tools          | Use                  |
| -------- | ---------------------- | -------------------- |
| Metrics  | Prometheus, CloudWatch | Track service health |
| Logs     | ELK, Datadog Logs      | Debug failure causes |
| Traces   | Jaeger, Zipkin         | Root-cause latency   |
| Alerting | PagerDuty, OpsGenie    | On-call notification |

---

## 🎯 FAANG Mock Interview Questions

---

### ❓ Question 1:

**"Your system must have 99.99% availability. How would you design it?"**

✅ Talking Points:

- Multi-AZ deployment
- Load-balanced replicas
- Auto-healing instances
- Redundant DB with failover
- Monitoring + alerts for failures

---

### ❓ Question 2:

**"Explain how you’d design a fault-tolerant microservice that depends on another slow or failing service."**

✅ Answer:

- Use circuit breaker around the dependency
- Timeout and retry logic with backoff
- Return fallback response (graceful degradation)
- Isolate dependency calls via queue or bulkhead

---

### ❓ Question 3:

**"What’s the difference between HA and FT?"**

✅ Sample Answer:

- HA ensures **uptime** via redundancy + load balancing
- FT ensures **correctness** despite partial failures
- FT is a _subset_ or an _implementation_ strategy for HA

---

## 💡 Real-World Systems

| Company | Practice                                               |
| ------- | ------------------------------------------------------ |
| Netflix | Chaos Monkey for resilience testing                    |
| Amazon  | Zones + regions + retries everywhere                   |
| Google  | Borg + SRE handbooks define HA practices               |
| Uber    | Multi-region service mesh + graceful fallback patterns |

---

## ✅ Summary Checklist

| Area                 | Best Practice                                     |
| -------------------- | ------------------------------------------------- |
| Load Balancing       | Use L7/L4 with health checks                      |
| Redundancy           | N+1 replicas, read replicas, cross-region backups |
| Auto-Healing         | Kubernetes, ECS, VM orchestration                 |
| Circuit Breaker      | Use Resilience4j, handle timeouts + retries       |
| Graceful Degradation | Fallback paths for core services                  |
| Monitoring           | Alert on high error rate or latency spikes        |

---
