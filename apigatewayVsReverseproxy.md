# Load balancer Vs Reverse Proxy Vs Forward Proxy Vs API Gateway

## 🧭 System Design Component Comparison: Load Balancer vs Reverse Proxy vs Forward Proxy vs API Gateway

| Feature / Aspect               | 🧊 Load Balancer                        | 🔁 Reverse Proxy                             | 🔄 Forward Proxy                           | 🧠 API Gateway                             |
| ------------------------------ | --------------------------------------- | -------------------------------------------- | ------------------------------------------ | ------------------------------------------ |
| **Acts on behalf of**          | Backend servers                         | Backend servers                              | Clients                                    | Microservices / APIs                       |
| **Traffic Direction**          | Client ➝ LB ➝ Backend                   | Client ➝ Proxy ➝ Backend                     | Client ➝ Proxy ➝ Internet                  | Client ➝ Gateway ➝ Services                |
| **Main Purpose**               | Distribute traffic across servers       | Intercept, modify, route incoming traffic    | Hide client identity, filter outbound      | Manage, secure, and route API requests     |
| **Used For**                   | Load distribution, HA, fault tolerance  | SSL termination, routing, caching, auth      | Privacy, content filtering, access control | API versioning, throttling, monitoring     |
| **Handles Static Content**     | ❌ (rarely)                             | ✅ Yes (optional)                            | ❌                                         | ❌                                         |
| **Handles Dynamic Routing**    | ❌ (limited)                            | ✅ Yes                                       | ❌                                         | ✅ Yes                                     |
| **Authentication**             | ❌                                      | ✅ (with plugins/modules)                    | ❌                                         | ✅ OAuth2, JWT, API keys                   |
| **Rate Limiting / Throttling** | ❌                                      | ❌ (manual config)                           | ❌                                         | ✅ Built-in                                |
| **Caching Support**            | ❌                                      | ✅ Yes                                       | ✅ (some forward proxies cache)            | ✅ (some support it)                       |
| **SSL Termination**            | ✅ Optional                             | ✅ Common use case                           | ❌                                         | ✅ Common use case                         |
| **Common Placement**           | Between clients and app servers         | Between clients and app servers              | Between clients and internet               | At the edge of services (entry point)      |
| **Layer (OSI Model)**          | L4 (TCP) / L7 (HTTP)                    | L7 (HTTP/S)                                  | L3/L4 or L7                                | L7 (HTTP/HTTPS)                            |
| **Popular Tools**              | AWS ELB, HAProxy, NGINX, Envoy          | NGINX, Apache, Traefik, Caddy                | Squid, Privoxy, CCProxy                    | Kong, Apigee, AWS API Gateway, Istio       |
| **Common Use Case**            | Distribute traffic across N app servers | Terminate SSL, route to service1 or service2 | Employee internet access filter            | Expose and secure public microservice APIs |

---

## 🔄 Quick Visual

```text
Client --> Load Balancer --> App Server Pool
Client --> Reverse Proxy  --> Internal Services
Client --> Forward Proxy  --> External Internet
Client --> API Gateway    --> Microservices
```
