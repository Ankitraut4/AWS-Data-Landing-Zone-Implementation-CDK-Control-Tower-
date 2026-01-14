# AWS Data Landing Zone Implementation (CDK + Control Tower)

## Overview

This repository demonstrates the **implementation and customization of an AWS Data Landing Zone (DLZ)** using **AWS CDK** and **AWS Control Tower** principles to establish a **secure, compliant, multi-account AWS foundation**.

The project focuses on **platform-level responsibilities** typically owned by a Cloud Center of Excellence (CCoE), including **account governance, security guardrails, identity management, networking, logging, and cost controls**.  
It intentionally **excludes application development and data pipelines**.

This implementation is suitable for **regulated enterprise environments** (e.g., payments and financial services) where **auditability, least privilege, and controlled change management** are mandatory.

---

## Project Context

- This project **does not claim authorship** of the DLZ framework.
- It demonstrates **practical usage, configuration, and extension** of an **open-source DLZ CDK construct**.
- The goal is to show **hands-on platform engineering experience**, not framework development.

---

## My Contributions

- Implemented a **multi-account AWS landing zone** using AWS CDK aligned with **AWS Control Tower best practices**
- Defined organizational structure using **AWS Organizations**, OUs, and environment separation (dev / prod)
- Configured **Service Control Policies (SCPs)** to restrict high-risk services and enforce preventative guardrails
- Designed **secure VPC baselines** with non-overlapping CIDR ranges, private subnets, NAT access, and bastion hosts
- Enabled **centralized audit logging and security visibility** using AWS Config, Security Hub, and Control Tower controls
- Configured **IAM Identity Center (SSO)**, permission sets, and **permission boundaries** to enforce least-privilege access
- Implemented **AWS Budgets and notifications** for cost governance and proactive spend monitoring
- Deployed infrastructure using **dependency-aware, staged (wave-based) deployments** to reduce blast radius

---

## Architecture Summary

### Accounts
- Management account
- Workload accounts (Development, Production)
- Clear isolation between environments

### Governance
- AWS Organizations
- Service Control Policies (deny lists, root restrictions)
- Tag policies for ownership and cost tracking

### Security & Compliance
- AWS Control Tower controls (preventive & detective)
- AWS Config rules
- AWS Security Hub standards
- Centralized CloudTrail logging

### Networking
- Non-overlapping VPC CIDR allocation
- Private subnets with controlled egress
- NAT Gateways or instances
- Bastion hosts with AWS SSM access
- Optional VPC peering for controlled cross-account communication

### Identity & Access
- IAM Identity Center (SSO)
- Permission sets per role
- Permission boundaries to prevent privilege escalation

---

## Getting Started

### Prerequisites

- Existing or new **AWS Organization**
- AWS Control Tower enabled
- AWS CDK (TypeScript or Python)
- Pre-created AWS accounts in the target OU (per SOP)

---

## Usage (TypeScript)

```bash
npm install aws-data-landing-zone

import { App } from 'aws-cdk-lib';
import { DataLandingZone, Defaults } from 'aws-data-landing-zone';

const app = new App();

new DataLandingZone(app, {
  regions: {
    global: Region.EU_WEST_1,
    regional: [Region.US_EAST_1],
  },
  budgets: [
    ...Defaults.budgets(100, 20, {
      emails: ['security@org.com'],
    }),
  ],
  denyServiceList: [
    ...Defaults.denyServiceList(),
    'ecs:*',
  ],
});

```

## Usage (Python)
```
pip install aws-data-landing-zone

import aws_cdk as cdk
import aws_data_landing_zone as dlz

app = cdk.App()

dlz.DataLandingZone(
    app,
    regions=dlz.DlzRegions(
        global_=dlz.Region.EU_WEST_1,
        regional=[dlz.Region.US_EAST_1],
    ),
)
```
## Security & Compliance Considerations

This project emphasizes **defense-in-depth**:

- Centralized audit logging across all accounts
- Preventive guardrails enforced via Service Control Policies (SCPs)
- Least-privilege access using IAM Identity Center and permission boundaries
- Network isolation using private subnets and controlled ingress/egress
- Cost governance using AWS Budgets and tagging policies

> **Note:** This project provides foundational infrastructure only.  
> Regulatory compliance depends on organizational policies, operational controls, and ongoing governance.

---

## Intended Audience

- Cloud / Platform Engineers
- Infrastructure Engineers
- Security Engineers
- Cloud Center of Excellence (CCoE) teams

**Out of scope**
- Application development
- Data engineering pipelines
- Data science workloads

---

## Core Principles

- Opinionated but configurable defaults
- Security and governance first
- Automation with controlled manual approvals
- Simplicity over unnecessary abstraction
- Clear separation of platform vs application concerns

---

## Integrated AWS Services

- AWS Organizations
- AWS Control Tower
- Service Control Policies (SCPs)
- AWS Config
- AWS Security Hub
- AWS Budgets
- IAM Identity Center (SSO)
- Amazon VPC, NAT, and SSM
- AWS Lake Formation (optional configuration)

---

## How It Works

- All accounts and resources are defined via a **single CDK construct**
- Accounts are classified as **development or production**
- Resources are deployed using **global and regional deployment waves**
- Global resources (IAM, policies) deploy first
- Regional resources (VPCs, networking) deploy afterward
- Production deployments can require **manual approval**

This deployment model minimizes risk and supports **enterprise change management**.

---

## Attribution

This project uses the **open-source Data Landing Zone (DLZ) CDK construct**  
maintained by **DataChef and the community**.

This repository demonstrates **implementation and customization** of the construct  
for learning and portfolio purposes.



