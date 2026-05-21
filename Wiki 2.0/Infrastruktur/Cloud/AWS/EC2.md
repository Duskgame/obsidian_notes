# AWS EC2

[AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/index.html) | [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)

Elastic Compute Cloud (EC2) is AWS's virtual machine service — the most fundamental compute building block. You rent a server, choose the OS and hardware, and have full control over it.

---

## What It Is

An EC2 **instance** is a virtual machine running on AWS hardware. You choose:
- **OS** (Amazon Linux, Ubuntu, Windows, etc.)
- **Instance type** (CPU, memory, storage, network)
- **[[AWS VPC|VPC]] and subnet** (where in the network it runs)
- **Storage** (EBS volume attached to the instance)
- **[[AWS IAM|IAM]] instance profile** (what AWS services it can access)

---

## Instance Type Families

Instance types follow the pattern `family.size` (e.g. `t3.micro`, `c6i.large`):

| Family | Optimised for | Example use |
|---|---|---|
| `t` | General purpose, burstable | Dev environments, small apps |
| `m` | General purpose, balanced | Web servers, mid-tier apps |
| `c` | Compute intensive | Data processing, batch jobs |
| `r` | Memory intensive | Large in-memory caches, databases |
| `p` / `g` | GPU | Machine learning, rendering |
| `i` | Storage intensive (NVMe) | High-throughput databases |

Sizes: `nano < micro < small < medium < large < xlarge < 2xlarge ...`

---

## EC2 in the Compute Hierarchy

EC2 is the baseline — everything else is an abstraction on top:

| Service | What it abstracts |
|---|---|
| **EC2** | Raw VM — you manage everything |
| **ECS** | Runs Docker containers on EC2; AWS manages the scheduler |
| **EKS** | Runs [[Kubernetes]] on EC2; AWS manages the control plane |
| **Lambda** | Runs functions on EC2; AWS manages everything |
| **RDS** | Runs a database on EC2; AWS manages OS, backups, patching |

---

## When to Use EC2

| Use EC2 | Use a managed/serverless alternative |
|---|---|
| Need full OS control | Just need to run a function or container |
| Long-running processes | Event-driven, short-lived tasks |
| Specific runtime or software not in containers | Standard application workloads |
| Consistent, predictable heavy load | Spiky or unpredictable traffic |

---

## Pricing Models

| Model | Description | Best for |
|---|---|---|
| **On-Demand** | Pay per second, no commitment | Dev/test, unpredictable load |
| **Reserved** | 1–3 year commitment, up to 72% discount | Stable, predictable baseline |
| **Spot** | Bid on spare capacity, up to 90% discount | Fault-tolerant batch jobs |
| **Savings Plans** | Flexible commitment across instance families | Most production workloads |

---

## Key Related Services

| Service | What it adds to EC2 |
|---|---|
| **EBS** (Elastic Block Store) | Persistent disk storage attached to an instance |
| **AMI** (Amazon Machine Image) | Snapshot of an instance used to launch new identical instances |
| **Auto Scaling Group** | Automatically adds/removes EC2 instances based on demand |
| **Elastic IP** | Static public IP address that can be reassigned between instances |

---

## Related Topics

- [[AWS VPC]] — EC2 instances always run inside a VPC subnet
- [[AWS IAM]] — EC2 instances assume IAM roles via instance profiles
- [[Kubernetes]] — EKS runs Kubernetes workloads on EC2 worker nodes
- [[docker]] — EC2 is the typical host for self-managed Docker deployments
- [[Serverless]] — Lambda is the serverless alternative to EC2 for stateless functions
