# AWS Cost Estimate

## Baseline Cost

### EC2

t3.medium × 4

$0.0416 × 4 × 720

= $119.81

### RDS Primary

db.r6g.large

$0.182 × 720

= $131.04

### Read Replicas

db.r6g.large × 2

$0.182 × 2 × 720

= $262.08

### Redis

cache.r6g.large × 3

$0.166 × 3 × 720

= $358.56

### ALB

≈ $56.20

### CloudFront

≈ $85

### SQS

≈ $12

---

## Total Baseline Cost

≈ $1,024/month

---

## World Cup Peak Cost

### Extra EC2

t3.2xlarge × 20

$0.3328 × 20 × 4

= $26.62

### Bigger RDS

db.r6g.4xlarge

$1.027 × 4

= $4.11

### CloudFront Surge

50TB

= $425

---

## Total Peak Extra Cost

≈ $455

---

## Business Justification

Outage loss:

₹4.2 crore/minute

45 minutes outage:

₹189 crore

Infrastructure cost:

$1,024/month

Result:

Infrastructure investment is significantly cheaper than downtime.