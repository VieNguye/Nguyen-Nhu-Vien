Provide your solution here:

## 2. AWS Services and Their Role

| Service                   | Purpose                                                       |
|---------------------------|---------------------------------------------------------------|
| API Gateway               | Handles incoming trading API requests, secures endpoints with WAF. |
| ALB (Application Load Balancer) | Distributes traffic across multiple Kubernetes nodes for high availability. |
| Amazon EKS (Kubernetes)   | Runs the trading engine, order matching service, and other microservices in containers. |
| AWS Aurora PostgreSQL     | Stores trades, order books, and historical transaction data with high availability. |
| ElastiCache (Redis)       | Caches frequently accessed order book data to reduce database load. |
| Kafka / SQS               | Handles order queueing, ensuring reliable trade execution. |
| Amazon S3                 | Stores logs, snapshots, and trade history for compliance. |
| CloudWatch & X-Ray        | Monitors system health and optimizes performance. |

# 3. Scaling Strategy

## Short-Term Scaling (Meeting 500 RPS)
- Auto-scaling Kubernetes pods based on CPU/memory.
- Read replicas for Aurora to distribute read traffic.
- Redis ElastiCache for low-latency order book data.

## Long-Term Scaling (Beyond 500 RPS)
- Sharding Aurora PostgreSQL based on trading pairs (e.g., BTC/USD, ETH/USD).
- Multi-region setup using AWS Global Accelerator to reduce latency.
- Decentralized Matching Engines deployed across multiple regions.