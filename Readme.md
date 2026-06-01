# Swiggy Scale Simulation

A scale engineering exercise analysing how a Swiggy-like monolith fails under a World Cup traffic spike.

## Documents

| File | Purpose |
|--------|---------|
| FAILURE-CASCADE.md | Failure analysis |
| ARCHITECTURE.md | Redesigned architecture |
| COST-ESTIMATE.md | AWS pricing |
| RUNBOOK.md | Incident response |

## Key Findings

- Peak traffic reaches 500,000 RPS.
- PostgreSQL pool fails first.
- Node crashes due to queued requests.
- Images alone can saturate network bandwidth.
- Async payments drastically reduce DB pressure.

## Architecture Overview

CloudFront + ALB + Node.js cluster + Redis + PgBouncer + PostgreSQL replicas + SQS payment workers.

## Technologies

- Node.js
- PostgreSQL
- Redis
- AWSgit add .
git commit -m "complete swiggy scale simulation assignment"
git push origin swiggy-scale
Project submission for Kalvium Scale Simulation Assignment.