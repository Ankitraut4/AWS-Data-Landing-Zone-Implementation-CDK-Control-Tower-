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

----------------------------------------------------------

## Usage (Python)



