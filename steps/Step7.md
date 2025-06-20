# 🛡️ Step 7: Security, Rate Limiting & Throttling

> No matter how well you design your backend, it must also be **secure, abuse-resistant**, and able to handle **spikes gracefully**.

---

## 🔹 1. Key Concepts

| Concept            | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| Authentication     | Verify **who** the user is (e.g., OAuth, JWT)                |
| Authorization      | Verify **what** the user can access (RBAC, ABAC)             |
| Rate Limiting      | Limit # of requests per user/IP in a time window             |
| Throttling         | Dynamically slow down or reject excess requests              |
| API Gateway        | Front door for APIs; applies security, limits, routing       |
| Token-based Access | Use JWTs or access tokens instead of sessions                |
| DDoS Protection    | Mitigate massive flood of requests (malicious or accidental) |

---

## 🔐 2. Authentication & Authorization

---

### ✅ Authentication (AuthN)

**Goal:** Prove **who** the user is.

| Method     | Description                       | Example Use                 |
| ---------- | --------------------------------- | --------------------------- |
| Basic Auth | Username:Password (base64)        | Dev APIs                    |
| API Keys   | Token per app/client              | Public APIs                 |
| OAuth2     | Third-party auth (Google, GitHub) | Sign in with Google         |
| JWT        | Self-contained signed tokens      | Stateless auth, mobile APIs |

---

### ✅ Authorization (AuthZ)

**Goal:** Control **what** the user can access or do.

#### Access Control Models

| Model | Example                                   |
| ----- | ----------------------------------------- |
| RBAC  | Role-based access (Admin, User, Viewer)   |
| ABAC  | Attribute-based (Region = "IN", Age > 18) |
| ACL   | Per-resource user access                  |

---

## 🚦 3. Rate Limiting

**Why?** Prevent abuse, protect downstream services, ensure fair usage.

---

### 🔹 Strategies

| Strategy       | Description                           | Example                        |
| -------------- | ------------------------------------- | ------------------------------ |
| Token Bucket   | User gets N tokens/sec. Burst allowed | API = 10 req/sec with burst 20 |
| Leaky Bucket   | Requests drain at constant rate       | Smooth flow, backpressure      |
| Fixed Window   | N requests allowed per minute         | 100 req/min reset every minute |
| Sliding Window | Time-weighted average (more accurate) | 100 req/min rolling window     |

---

### 📦 Where to Apply?

| Layer         | Example                         |
| ------------- | ------------------------------- |
| API Gateway   | NGINX, Kong, Amazon API Gateway |
| CDN Layer     | Cloudflare, Akamai              |
| Backend Logic | Custom app middleware           |
| Per User/IP   | Use Redis/DB to store counters  |

---

## 🔃 4. Throttling

> Dynamic defense mechanism to slow down or reject traffic during overload.

### When to Throttle:

- Downstream DB or queue is backlogged
- Traffic spike from a client
- Prevent cascading failure

### Techniques:

- Return HTTP 429 (Too Many Requests)
- Exponential backoff (retry with delay)
- Queue requests or degrade features

---

## 🧰 5. Tools & Technologies

| Purpose         | Tools/Tech                              |
| --------------- | --------------------------------------- |
| Auth / OAuth    | Auth0, Firebase Auth, AWS Cognito       |
| API Gateway     | Kong, NGINX, Envoy, AWS API Gateway     |
| Rate Limiting   | Redis counters, Envoy, Istio            |
| JWT Issuance    | `jsonwebtoken` (Node), Authlib (Python) |
| DDoS Protection | Cloudflare, AWS Shield, Fail2ban        |

---

## 🔐 Token-Based Access (JWT)

- Encoded JSON payload, signed (not encrypted)
- Contains user ID, roles, expiration
- Stateless (server doesn’t store session)
- Must verify signature on each request

### Structure:

```json
{
  "sub": "user123",
  "exp": 1680000000,
  "role": "admin"
}
```

## 💣 6. DDoS Protection

Why? Prevent system crash under massive automated traffic.

### Defenses:

- Use CDN + WAF (Cloudflare, Akamai)
- Block known bad IPs / geos
- Apply rate limits per IP/user
- Use circuit breakers in backend

---

## 🎯 FAANG Mock Interview Questions

---

### ❓ Question 1:

**"How would you design rate limiting for an API?"**

✅ **Answer:**

- Use **token bucket** algorithm
- Track request count per user/token in **Redis**
- Return `429 Too Many Requests` if limit exceeded
- Include `Retry-After` header in the response

---

### ❓ Question 2:

**"What's the difference between Authentication and Authorization?"**

✅ **Talking Points:**

- **Authentication (AuthN):** Verifies identity (e.g., login, tokens)
- **Authorization (AuthZ):** Verifies access permissions (e.g., what you can do)
- **Example:** You're logged in (AuthN), but can’t delete other users (AuthZ)

---

### ❓ Question 3:

**"How would you secure an internal API between services?"**

✅ **Sample Answer:**

- Use **mTLS (mutual TLS)** for service-to-service identity
- Use **JWTs with scopes/claims** to control access
- Apply **rate limiting** and **distributed tracing** for abuse/failure detection

---

### ❓ Question 4:

**"What happens if a JWT token is stolen? How do you protect against misuse?"**

✅ **Strategies:**

- Use short TTL (e.g., 15 minutes) for access tokens
- Use **refresh tokens + rotation**
- Store JWTs in **HttpOnly** cookies to avoid XSS
- Bind token to **IP address** or **browser fingerprint**

---

## 💡 Real-World Practices

| Company | Security Practices                                         |
| ------- | ---------------------------------------------------------- |
| Google  | **mTLS** + **zero-trust** between internal microservices   |
| Amazon  | **AWS IAM** roles + **Cognito** for user authentication    |
| GitHub  | **OAuth2** + fine-grained **scopes** for API access tokens |
| Stripe  | **JWT** + per-client rate limits + versioned APIs          |

---

## ✅ Summary Checklist

| Topic               | Best Practice                                       |
| ------------------- | --------------------------------------------------- |
| **Auth**            | Use **OAuth2** or **JWT** for stateless APIs        |
| **Rate Limiting**   | Use **token bucket** with **Redis** or **Envoy**    |
| **Throttling**      | Return **429**, apply **backoff**, protect services |
| **API Gateway**     | Terminate **TLS**, apply **limits**, route traffic  |
| **Token Storage**   | Use **HttpOnly cookies** or secure headers          |
| **DDoS Protection** | Use **CDN**, **WAF**, **IP blocking**, burst limits |
