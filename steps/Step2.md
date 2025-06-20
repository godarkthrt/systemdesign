# 🧩 Step 2: MVP Architecture (Minimum Viable Product Design)

> Once requirements are clear, design a simple yet functional architecture to bring the product to life.

---

## 🔹 1. Key Concepts

| Concept                        | Description                                                        |
| ------------------------------ | ------------------------------------------------------------------ |
| Monolithic Architecture        | Single codebase, one deployable unit (simple to start with)        |
| Stateless Services             | App servers don’t retain session data → easy to scale horizontally |
| Load Balancer                  | Distributes incoming traffic across multiple backend servers       |
| Relational Database            | Stores structured data (PostgreSQL, MySQL)                         |
| API Layer                      | Exposes endpoints for frontend & clients                           |
| Vertical vs Horizontal Scaling | Upgrade machine vs add more machines                               |

---

## 🔹 2. Detailed Breakdown

### 🧱 Core MVP Components

```
                    +------------------+
                    |   Frontend (UI)  |
                    +--------+---------+
                             |
                             v
                    +------------------+
                    |  API Gateway /   |
                    | Reverse Proxy    |  ← Nginx, API Gateway
                    +--------+---------+
                             |
                             v
     +-------------------------------+
     |         App Server            |  ← Handles business logic
     |   (Stateless, REST APIs)      |
     +-------------------------------+
                 |
                 v
     +-------------------------------+
     |     Relational Database       |  ← PostgreSQL / MySQL
     +-------------------------------+

```

---

### 📌 Key Design Choices

#### ✅ Monolith vs Microservices

| Monolith (MVP)         | Microservices (Scale Phase)        |
| ---------------------- | ---------------------------------- |
| Simple to build/deploy | Complex, more scalable             |
| Tight coupling         | Loose coupling                     |
| Ideal for MVP          | Ideal for scaling teams & features |

#### ✅ Stateless App Servers

- No session stored in app memory
- Sessions (if needed) stored in:
  - Cookies (JWT)
  - Redis (session store)
- Enables horizontal scaling

#### ✅ Load Balancer

- Routes traffic to healthy backend servers
- Can perform health checks
- Examples: Nginx, HAProxy, AWS ELB

#### ✅ RESTful API Layer

- Clear separation of frontend and backend
- Common practices:
  - `GET /users/:id`
  - `POST /photos`
  - Use HTTP status codes meaningfully

---

### 🔁 Scaling: Vertical vs Horizontal

| Scaling Type | Description                            | When to Use                |
| ------------ | -------------------------------------- | -------------------------- |
| Vertical     | Increase CPU/RAM on one server         | Quick fix, costly at scale |
| Horizontal   | Add more servers (with load balancing) | Preferred for scale + HA   |

---

## 🧠 Design Tradeoffs

| Decision          | Pros                    | Cons                              |
| ----------------- | ----------------------- | --------------------------------- |
| Monolith          | Simple dev & deploy     | Hard to scale parts independently |
| REST API          | Standardized, simple    | Verbose for real-time apps        |
| Relational DB     | ACID, structured schema | May not scale horizontally        |
| Stateless Servers | Easier scaling          | Needs external session management |

---

## 🎯 FAANG Mock Interview Questions

### Question 1:

**"Design an MVP version of Instagram. What components would you use to get it working for the first 10K users?"**

✅ Tips to Answer:

- Talk about frontend, backend (monolith), relational DB
- Use RESTful API
- Stateless app servers behind a load balancer
- Store images in a blob store (S3 or similar), and metadata in DB

---

### Question 2:

**"Why would you choose a monolithic architecture for your MVP instead of jumping straight to microservices?"**

✅ Sample Response:

> “Monoliths are easier to build and deploy early on. We can avoid network complexity, distributed transactions, and over-engineering. Once we hit clear scale pain points, we can gradually extract services.”

---

### Question 3:

**"What does stateless mean, and why is it important for scalability?"**

✅ Sample Response:

> “A stateless server doesn’t store any session or user data between requests. This allows any instance to serve any request, enabling horizontal scaling and simplifying load balancing.”

---

## 💡 Real-World Insight

| Company   | MVP Strategy                            |
| --------- | --------------------------------------- |
| Airbnb    | Monolith Rails app + PostgreSQL         |
| Instagram | Django monolith + S3 + PostgreSQL       |
| Dropbox   | Python monolith, REST API, MySQL        |
| Twitter   | Monolith → moved to services only later |

Most scaled companies started with monoliths, REST APIs, and RDBMS.

---

## ✅ Summary Checklist

| Component     | MVP Best Practice                             |
| ------------- | --------------------------------------------- |
| App Server    | Stateless, behind a load balancer             |
| Load Balancer | Basic round-robin + health checks             |
| DB            | Relational (PostgreSQL/MySQL)                 |
| API           | RESTful, documented, versioned                |
| Scaling       | Vertical (initially), Horizontal (eventually) |
| Deployment    | Single deploy unit, CI/CD optional            |

---
