# Failure Cascade Analysis - SwiftEats

## 1. Traffic Simulation Math

Total users notified = 180,000,000

CTR = 8%

Users opening app:

180,000,000 × 8%
= 14,400,000 users

Assume:
- 3 API calls per user
- 60 second spike window

Peak RPS:

(10,000,000 × 3) / 60

= 500,000 RPS

---

## 2. Component Capacity Numbers

### PostgreSQL

max_connections = 100

### Node.js

Capacity ≈ 12,000 RPS

### Payment Calls

Hold time:
200ms - 2000ms

Average:
800ms

### DB Pool Exhaustion

Connections held:

70% normal requests:
350 × 0.02 = 7

30% payment requests:
150 × 0.8 = 120

Total:
127 connections

Pool limit:
100

Result:
Pool exhausted around 394 RPS

---

## 3. Failure Cascade

### Failure 1: PostgreSQL Pool Exhaustion (CRITICAL)

Trigger:
~394 RPS

User Impact:
500 errors

Next Effect:
Node queues requests

---

### Failure 2: Node Event Loop Saturation (CRITICAL)

Trigger:
~12,000 RPS

User Impact:
Timeouts

Next Effect:
Memory growth

---

### Failure 3: Synchronous Payments (HIGH)

Trigger:
Heavy checkout traffic

User Impact:
Slow order placement

Next Effect:
DB connections remain occupied

---

### Failure 4: Promo Race Condition (HIGH)

Trigger:
Millions applying promo simultaneously

User Impact:
Oversold discounts

Next Effect:
Revenue loss

---

### Failure 5: Static Asset Saturation (CRITICAL)

Trigger:
Image traffic spike

User Impact:
App unavailable

Next Effect:
NIC fully saturated

---

## 4. Incident Timeline

T+0s  : Notification sent

T+3s  : DB pool exhausted

T+5s  : Node backlog begins

T+8s  : New DB requests rejected

T+10s : Payment timeouts

T+12s : Promo oversold

T+15s : NIC saturated

T+18s : Node crash

T+20s : Service unavailable

T+45m : Root cause identified

T+2h  : Recovery complete
