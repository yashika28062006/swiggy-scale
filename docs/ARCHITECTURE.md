# Redesigned Architecture

## Current Architecture

Users
 |
 v
Node.js Server
 |
 v
PostgreSQL

Weaknesses:

- Pool exhaustion
- No cache
- No CDN
- No load balancer
- Sync payments

---

## New Architecture

Users
 |
 v
CloudFront CDN
 |
 v
Application Load Balancer
 |
 +---- Node 1
 |
 +---- Node 2
 |
 +---- Node 3
 |
 +---- Node 4
 |
 v
Redis Cluster
 |
 v
PgBouncer
 |
 +---- PostgreSQL Primary
 |
 +---- Read Replica 1
 |
 +---- Read Replica 2
 |
 v
SQS Queue
 |
 v
Payment Workers

---

## Component Justification

| Component | Failure Prevented | How |
|------------|------------------|------|
| CloudFront | NIC Saturation | Serves images from edge |
| ALB | Single Point Failure | Distributes traffic |
| Redis | Pool Exhaustion | Caches reads |
| Redis Lock | Promo Race | Atomic updates |
| PgBouncer | DB Exhaustion | Reuses connections |
| Read Replicas | Read Contention | Split workload |
| SQS | Sync Payment Blocking | Async processing |
| Payment Workers | Checkout Delay | Background execution |git add docs/ARCHITECTURE.md


