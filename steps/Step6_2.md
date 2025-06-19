# 🧩 Step 6: Monitoring, Observability & Reliability

> "You can’t improve what you don’t measure."  
> Monitoring and observability let you understand your system’s health, performance, and reliability in real time.

---

## 🔹 1. Key Concepts

| Term          | Description                                                               |
| ------------- | ------------------------------------------------------------------------- |
| Monitoring    | Tracking system metrics and alerts for anomalies                          |
| Observability | Ability to understand _why_ something went wrong using logs, traces, etc. |
| SLI/SLO/SLA   | Metrics to define and measure service reliability                         |
| Logging       | Time-stamped records of system events or errors                           |
| Tracing       | Follows a request across multiple systems or services                     |
| Alerting      | Real-time notification on metric breaches or system failures              |

---

## 🔹 2. Detailed Breakdown

---

### 📊 Monitoring

**Goal:** Watch system-level metrics like:

- CPU, memory, disk I/O
- Error rate
- Request latency (p95, p99)
- DB connection pool usage
- Queue length
- Service uptime

#### 🧰 Tools:

- **Prometheus** (metrics collector + alerting)
- **Grafana** (dashboard & visualization)
- **Datadog / New Relic / CloudWatch** (full-stack SaaS observability)

---

### 🔎 Observability

**Goal:** Go beyond "is it working?" to **"why is it failing?"**

#### 🔹 3 Pillars of Observability:

| Pillar  | What It Shows                     | Tools                                          |
| ------- | --------------------------------- | ---------------------------------------------- |
| Logs    | What happened?                    | ELK (Elasticsearch, Logstash, Kibana), Fluentd |
| Metrics | What is happening? (quantitative) | Prometheus, CloudWatch, Datadog                |
| Traces  | How did it happen? (request flow) | Jaeger, Zipkin, OpenTelemetry                  |

✅ **Combine logs, metrics, and traces** to debug slowdowns, errors, and outages.

---

### 🧭 Distributed Tracing

**Why?** In microservices, a single user request may span 10+ services.

🔍 Tracing helps you:

- See where time is spent
- Spot bottlenecks
- Identify failing services

#### Example:

```
Request: GET /checkout
→ cart-service (50ms)
→ inventory-service (120ms)
→ payment-service (400ms) ← problem here
```

---

### 🛎️ Alerting

**Best Practice:** Alerts should be:

- **Actionable** (no noise)
- **Timely** (real-time)
- **Prioritized** (critical vs warning)

#### Alert Examples:

- 5xx error rate > 2% for 5 mins
- p99 latency > 2s
- DB CPU > 90% for 10 mins
- Kafka lag growing > 1k messages

#### Tools:

- PagerDuty
- Opsgenie
- Slack/Webhooks from Grafana, Prometheus, etc.

---

### 📐 SLI, SLO, SLA

| Term            | Meaning                               | Example                             |
| --------------- | ------------------------------------- | ----------------------------------- |
| SLI (Indicator) | What you measure                      | "p99 latency", "error rate"         |
| SLO (Objective) | Goal you aim to meet                  | "99.9% requests < 500ms"            |
| SLA (Agreement) | Business contract/penalty tied to SLO | "We refund if uptime < 99.5%/month" |

✅ Set realistic SLOs based on what your system _can actually deliver_.

---

## 🧠 Design Tradeoffs

| Decision             | Pros                                | Cons                                   |
| -------------------- | ----------------------------------- | -------------------------------------- |
| Real-time Monitoring | Fast response to incidents          | Can produce false alarms               |
| Logs vs Traces       | Logs are cheap, traces are detailed | Traces need instrumentation            |
| SLIs and SLOs        | Quantifies service quality          | Needs effort to set and track properly |

---

## 🎯 FAANG Mock Interview Questions

---

### ❓ Question 1:

**"What metrics would you monitor for a backend API?"**

✅ Answer:

- Request rate (RPS)
- Error rate (4xx, 5xx)
- Latency (p50, p95, p99)
- DB performance (query time, locks)
- Memory/CPU
- Queue length (if async jobs)

---

### ❓ Question 2:

**"What’s the difference between monitoring and observability?"**

✅ Talking Points:

- Monitoring: You define _what_ to watch
- Observability: You design systems that help you answer _why_ something failed
- Observability is proactive; monitoring is reactive

---

### ❓ Question 3:

**"Explain what a p99 latency spike might indicate."**

✅ Sample Answer:

> “If p99 latency increases, it means the slowest 1% of requests are much slower — possibly due to DB lock, GC pause, network congestion, or downstream service failure.”

---

### ❓ Question 4:

**"What would you do if your service is receiving traffic but returning 5xx errors?"**

✅ Sample Debug Plan:

- Check recent deploys (rollback if needed)
- Look at logs for stack traces
- Review DB health (e.g., connection pool exhaustion)
- Use tracing to find where requests are failing

---

## 💡 Real-World Practices

| Company  | Observability Stack                        |
| -------- | ------------------------------------------ |
| Netflix  | Atlas for metrics, internal tracing system |
| Uber     | M3 (custom metrics system), Jaeger         |
| LinkedIn | Kafka + Pinot for real-time monitoring     |
| Airbnb   | ELK + Prometheus + Grafana stack           |

---

## ✅ Summary Checklist

| Area       | Best Practice                                   |
| ---------- | ----------------------------------------------- |
| Metrics    | Track latency, error rates, system health       |
| Logging    | Centralized, structured logs (JSON, timestamps) |
| Tracing    | Use OpenTelemetry or Jaeger                     |
| Alerting   | Use thresholds and actionable alerts            |
| SLIs/SLOs  | Define and track reliability objectives         |
| Dashboards | Visualize key metrics with Grafana/Datadog      |

---

## ➡️ Next Step: Step 7 – Security, Rate Limiting, Throttling

We’ll cover:

- OAuth, JWT, API security
- Rate limits to protect backend
- DDoS protection, auth flows
- Role-based access control
