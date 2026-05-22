# Public Cloud

[AWS – What is Cloud Computing?](https://aws.amazon.com/what-is-cloud-computing/) | [Google Cloud – Cloud Computing Overview](https://cloud.google.com/learn/what-is-cloud-computing)

A public cloud is computing infrastructure (servers, storage, networking, databases, services) owned and operated by a third-party provider, shared across many customers, and accessed over the internet. You pay for what you use without owning or managing any hardware.

---

## Key Characteristics

**Pay-as-you-go** — no upfront hardware investment. Pay only for what you consume.

**On-demand scaling** — spin up or down in minutes. A single server or a thousand.

**Shared infrastructure** — workloads from many customers run on the same physical hardware, isolated via virtualisation and IAM.

**Managed by the provider** — physical security, hardware maintenance, power, cooling, and hardware failure are the provider's responsibility.

---

## Major Providers

| Provider | Full name | Known for |
|---|---|---|
| **AWS** | Amazon Web Services | Widest service breadth, most mature |
| **GCP** | Google Cloud Platform | Data/ML tooling, Kubernetes (created here) |
| **Azure** | Microsoft Azure | Enterprise/Windows integration |

---

## Deployment Models

| Model | Infrastructure owner | Shared? | Example |
|---|---|---|---|
| **Public cloud** | Provider (AWS, GCP…) | Yes | AWS EC2 |
| **Private cloud** | You or a colocation facility | No | On-premise VMware |
| **On-premises** | You | No | Your own server rack |
| **Hybrid cloud** | Mix of the above | Partial | On-prem + AWS for burst |

**Hybrid cloud** connects private/on-premises infrastructure with a public cloud — common in large enterprises that keep sensitive data on-premise while using cloud for scaling or less sensitive workloads.

---

## Core Service Categories

| Category | What it means | AWS example | GCP example |
|---|---|---|---|
| **IaaS** – Infrastructure as a Service | Raw compute, storage, networking | EC2, S3 | Compute Engine, GCS |
| **PaaS** – Platform as a Service | Managed runtime, you bring code | Elastic Beanstalk | App Engine, Cloud Run |
| **FaaS** – Functions as a Service | Run code without managing servers | Lambda | Cloud Functions |
| **SaaS** – Software as a Service | Full application, no infra at all | — | Google Workspace |

---

## Related Topics

- [[Cloud Native]] — architectural principles for building applications that run on public cloud
- [[AWS]] — Amazon's public cloud, most widely used
- [[GoogleCloud]] — Google's public cloud, used in SAKE and bonprix infrastructure
- [[Serverless]] — running functions without managing servers; fully managed by the cloud provider
- [[Kubernetes]] — container orchestration, often used on public cloud
