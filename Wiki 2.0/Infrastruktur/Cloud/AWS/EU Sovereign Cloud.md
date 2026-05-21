# AWS EU Sovereign Cloud

[AWS EU Sovereign Cloud Announcement](https://aws.amazon.com/de/blogs/security/aws-european-sovereign-cloud-overview/) | [AWS GDPR Center](https://aws.amazon.com/compliance/gdpr-center/)

The AWS European Sovereign Cloud is a separate AWS infrastructure region designed specifically for EU customers who require data to remain within the EU and under EU-resident operational control — primarily to address GDPR compliance and concerns about US government access under the CLOUD Act.

---

## Why It Exists

Standard AWS regions (including `eu-central-1` Frankfurt) are operated by Amazon Web Services Inc., a US company. The **US CLOUD Act (2018)** allows US authorities to compel US companies to hand over data stored anywhere in the world — including data stored on EU servers.

This creates a compliance conflict for:
- European government agencies
- Healthcare organisations (medical data)
- Financial institutions
- Any organisation subject to strict EU data sovereignty requirements

---

## What It Offers

| Feature | Description |
|---|---|
| **Data residency** | All data stored and processed exclusively in EU (initially Germany) |
| **EU-resident operators** | AWS staff managing the infrastructure are EU residents subject to EU law |
| **Separate legal entity** | Operated independently from US AWS to limit US legal reach |
| **GDPR alignment** | Designed to meet EU regulatory and data protection requirements |
| **Regulatory certifications** | Targeting BSI C5, ISO 27001, and sector-specific EU certifications |

---

## Residual Risks

Despite the separation, significant risks and open questions remain:

### 1. Ultimate US Ownership
AWS is ultimately owned by Amazon (a US company). No matter how the subsidiary is structured, the legal separation has not been tested in court. Whether US authorities could compel the EU entity is unresolved.

### 2. CLOUD Act Not Fully Neutralised
The structural separation is designed to limit CLOUD Act exposure, but legal experts disagree on whether it is sufficient. No definitive ruling exists.

### 3. Feature Parity Gap
The Sovereign Cloud region will not have all AWS services from day one. Organisations may be constrained compared to standard regions.

### 4. Higher Cost
Additional compliance infrastructure and operational requirements make Sovereign Cloud more expensive than equivalent standard region deployments.

### 5. Vendor Lock-in
Deep integration with AWS services creates dependency on a single US-owned provider. Migrating away later is expensive and complex.

---

## Alternatives for Full EU Sovereignty

For organisations requiring stronger sovereignty guarantees:

| Provider | Origin | Notes |
|---|---|---|
| IONOS Cloud | Germany | EU-owned, GDPR-native |
| OVHcloud | France | EU-owned, strong sovereignty positioning |
| Open Telekom Cloud | Germany (Telekom) | EU-owned, BSI C5 certified |
| Hetzner | Germany | Simple, cost-effective, EU-owned |

These providers have no US ownership and are not subject to the CLOUD Act.

---

## Summary

| | Standard AWS Region | EU Sovereign Cloud |
|---|---|---|
| Data location | EU possible | EU guaranteed |
| Operators | Global AWS staff | EU residents only |
| CLOUD Act exposure | High | Reduced (not eliminated) |
| Feature availability | Full | Limited (initially) |
| Cost | Standard | Higher |
| US ownership | Yes | Yes (separate entity) |

---

## Related Topics

- [[AWS IAM]] — access control remains critical regardless of which region is used
- [[AWS VPC]] — network isolation within the Sovereign Cloud follows the same VPC model
- [[Cloud Native]] — sovereignty concerns influence architecture decisions for regulated workloads
