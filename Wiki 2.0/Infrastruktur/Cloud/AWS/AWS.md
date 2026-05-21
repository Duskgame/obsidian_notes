# AWS

[AWS Documentation](https://docs.aws.amazon.com/) | [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)

Amazon Web Services — the leading cloud platform. Notes are split into general cloud concepts and AWS-specific service notes.

---

## General Cloud Concepts

- [[Serverless]] — compute model where you run functions without managing servers
- [[Cloud Native]] — designing applications to exploit cloud capabilities
- [[Kubernetes]] — container orchestration platform (EKS on AWS)
- [[Load Balancer]] — traffic distribution across backend instances (ALB/NLB on AWS)
- [[API Gateway]] — managed API entry point for routing, auth, rate limiting
- [[Database Partitioning]] — splitting large tables across physical partitions
- [[Observability]] — logs, metrics, and traces for operating distributed systems

---

## AWS Services

### Compute
- [[AWS Lambda]] — serverless function execution
- [[AWS EC2]] — virtual machines (the base compute unit)

### Networking & Security
- [[AWS VPC]] — isolated private network; subnets, security groups, gateways
- [[AWS IAM]] — identity and access management; users, roles, policies

### Messaging
- [[AWS SQS]] — managed message queue (point-to-point)
- [[AWS SNS]] — managed pub/sub messaging (broadcast)

### Infrastructure as Code
- [[AWS CDK]] — define AWS infrastructure in TypeScript/Python/Java/Kotlin

### Compliance & Sovereignty
- [[EU Sovereign Cloud]] — AWS infrastructure for EU data sovereignty requirements

---

## Distributed Systems Patterns (cloud-context)

- [[Circuit Breaker]] — protect against cascading failures from a failing dependency
- [[Saga Pattern]] — manage long-running transactions across multiple services
- [[Messaging Patterns]] — async request-response, fan-out, pub/sub, webhooks, topic bus
