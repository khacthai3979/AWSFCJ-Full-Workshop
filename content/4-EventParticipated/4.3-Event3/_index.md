---
title: "AWS Perimeter Protection Workshop – Strengthening Security with CloudFront & WAF"
date: "2025-11-18"
chapter: false
weight: 3
pre: "<b>4.3.</b>"
---

# AWS Perimeter Protection Workshop – Strengthening Security with CloudFront & WAF

## Event Overview
On **November 18th**, I attended the **AWS Perimeter Protection Workshop** at the AWS Vietnam office, focusing on perimeter security for web applications using **Amazon CloudFront** and **AWS WAF**.  
The session provided a lot of **hands-on knowledge**, new insights, and live demos that helped me understand in depth how to protect applications from the **Edge to the Origin**.

The workshop was led by:
- **Nguyễn Gia Hưng** – Head of Solutions Architect, AWS Vietnam

---

## CloudFront – Protecting from Edge to Origin

### New Pricing Model
One of the most notable updates is the **new subscription-based pricing model**, which eliminates the concern of “unexpected bill spikes”:

| Plan      | Price          |
|-----------|-----------------|
| Free      | $0              |
| Pro       | $15/month       |
| Business  | $200/month      |
| Premium   | $1,000/month    |

This model benefits **startups**, **SMEs**, and **enterprise workloads**, ensuring **predictable cost** and easier budgeting.

---

### Performance & Cost Optimization
Some real-world benchmark results shared during the workshop:

- **63% reduction** in Load Balancer cost  
- **80% reduction** in EC2 origin CPU utilization  
- **HTTP/3 (QUIC)** with **11 parallel downloads**  
- **Automatic HTTP compression** reduces file size by **82%**  

This proves that **moving caching logic to the Edge** not only reduces backend load, but also improves performance.

---

### New CloudFront Features

#### VPC Origin
- Allows hosting **ALB/EC2 inside Private Subnets**
- CloudFront creates **ENI** and tunnels traffic directly
- The origin **does not expose to the public internet**
- Critical for **financial systems**, banking workloads

#### Mutual TLS (mTLS – coming soon)
- Client certificate verification at CloudFront layer
- Very important for **Financial APIs**, **Open Banking**, **B2B integrations**

---

## AWS WAF – Comprehensive Application Protection

### Threat Landscape
The workshop highlighted the increasing sophistication of modern attacks:

- **DDoS attacks increased by 29% YoY**  
- **AI bot surge increased by 155%** (GPT, Claude, OpenAI Agents…)  

This requires a **multi-layer and multi-tier protection model**, not just blocking at Layer 7.

---

### Core Protection Components

#### Rate-based Rules
- Limit request frequency
- Protect against flood traffic & brute force attacks

#### Anti-DDoS AMR (Automatic Mitigation Rules)
- Automatic detection & real-time mitigation within seconds
- No manual intervention needed

#### Bot Control
- Detects **Evasive Bots**
- Using **client interrogation** and **fingerprinting**

#### AWS Shield Advanced
- Full DDoS protection
- **Automatic mitigation**
- **Cost Protection**: AWS **reimburses costs caused by DDoS traffic**

---

## Recommended Pattern for WAF Rule Ordering

The priority ordering of WAF rules directly impacts **performance**, **cost**, and **false positive rate**.  
Recommended order:

1. **Allowed IPs**  
2. **Blocked IPs**  
3. **Anti-DDoS AMRs**  
4. **Rate limiting**  
5. **Anonymous IPs / IP Reputation**  
6. **Core rules** (managed rulesets)
7. **False positive exceptions**  
8. **Bot/Fraud rules**  

Correct ordering ensures optimal processing efficiency and lower WAF cost.

---

## Key Best Practices

### End-to-end Visibility
- **CloudWatch RUM**  
- **AWS Internet Monitor**  
- **Infrastructure Monitoring**  

Ensures visibility from **Client → Edge → Origin**.

### Maximize Caching Efficiency
- Normalize cache keys  
- Cache dynamic content with **short TTL**  
- Cache error responses to reduce backend load  

### Block Unwanted Requests
- AWS WAF  
- Malicious pattern blocking  
- Rate limiting strategies  

### Offload Business Logic
Use **CloudFront Functions** for:
- CORS handling
- Redirects
- Cookie settings
- Header injection

This reduces logic handled by the backend.

### Automatic Failover
- **Route 53 Health-based Routing**  
- **CloudFront Origin Groups**  

Ensures **High Availability (HA)** when the origin fails.

---

## Conclusion
The workshop helped me understand how to **extend protection to the Edge**, optimize cost, improve performance, and build a **multi-layer security architecture**.  
What impressed me most was how CloudFront and WAF integrate with Shield, Bot Control, and Route 53 to form a **complete perimeter protection architecture**.

Applying these best practices ensures that systems are **secure**, while also **optimizing operational cost** and improving resilience.
