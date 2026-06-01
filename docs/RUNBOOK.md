# Incident Runbook

## Step 1 - Detect

| Metric | Threshold | Severity |
|----------|-----------|-----------|
| ALB 5xx | >5% | Critical |
| DB Connections | >80% | Warning |
| CPU | >80% | Warning |
| Redis Memory | >75% | Warning |
| SQS Depth | >10000 | Warning |
| P99 Latency | >2s | Critical |

---

## Step 2 - Triage

Check in this order:

1. RDS Connections
   - Red → DB issue

2. EC2 CPU
   - Red → Compute issue

3. Redis Cache Miss
   - Red → Cache issue

4. SQS Queue Depth
   - Red → Payment issue

---

## Step 3 - Respond

### DB Exhaustion

Action:
Increase pool size and restart PgBouncer

Success:
Connections < 80%

Team:
Database On-Call

---

### Compute Saturation

Action:
Scale EC2 Auto Scaling Group

Success:
CPU < 60%

Team:
Platform Team

---

### Redis Miss Spike

Action:
Warm cache

Success:
Cache hit > 80%

Team:
Backend Team

---

### Payment Queue Backup

Action:
Scale payment workers

Success:
Queue depth decreasing

Team:
Payments Team

---

## Step 4 - Rollback

Conditions:

- 5xx > 20%
- Not improving
- Recent deployment suspected

Command:

aws ecs update-service \
 --cluster swiggy-prod \
 --service api \
 --task-definition PREVIOUS_STABLE

Warning:
Never rollback database schema.

---

## Step 5 - Postmortem

### Timeline

Document every event with timestamps.

### Root Cause

Identify deepest technical reason.

### Impact

Duration, users affected, revenue loss.

### What Worked

Successful mitigation actions.

### Action Items

| Action | Owner | Due Date |
|----------|---------|---------|
| Example | Platform Team | DD/MM/YYYY |