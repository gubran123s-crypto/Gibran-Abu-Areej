# Gibran-Abu-Areej

# Implementing Attribute-Based Access Control (ABAC) in AWS

## Student Information
- **Name:** Gibran Abu Areej  
- **Model Chosen:** Attribute-Based Access Control (ABAC)  
- **AWS Region:** Europe (Stockholm) – `eu-north-1`

---

## 1. Introduction
This project demonstrates the implementation of the **Attribute-Based Access Control (ABAC)** model within **Amazon Web Services (AWS)**.  
ABAC is a modern and flexible access control mechanism where permissions are granted based on **attributes (tags)** associated with users and resources rather than static roles.

The main objective of this project is to show how **IAM principal tags** and **AWS resource tags** can be combined to dynamically control access to cloud resources such as **EC2**, **S3**, and **RDS**.

---

## 2. Why ABAC?
ABAC was selected instead of DAC or RBAC because:

- Scales efficiently in large environments
- Reduces the number of IAM policies
- Enables dynamic, tag-based access decisions
- Permission changes can be done simply by updating tags
- Improves security granularity and manageability

---

## 3. Architecture Overview
The AWS environment consists of the following components:

- EC2 Instances for **Development** and **Production**
- S3 Buckets for **Development** and **Production** data
- Amazon RDS (MySQL) for **Production Finance** data
- IAM Users with attribute-based tags
- A centralized IAM policy enforcing ABAC rules

All access decisions are made dynamically using **tag comparisons** between users and resources.

---

## 4. Resources Created

### 4.1 EC2 Instances

| Instance Name | Environment | Department   | Classification |
|--------------|------------|--------------|----------------|
| dev-ec2-1    | Development| Engineering  | Internal       |
| dev-ec2-2    | Development| Engineering  | Internal       |
| prod-ec2-1   | Production | Engineering  | Confidential   |
| prod-ec2-2   | Production | Finance      | Confidential   |

---

### 4.2 S3 Buckets

| Bucket Name         | Environment | Department  | Classification |
|---------------------|------------|-------------|----------------|
| dev-data-gibran     | Development| Engineering | Internal       |
| prod-data-gibran    | Production | Finance     | Confidential   |

---

### 4.3 RDS Database

| Property        | Value |
|-----------------|-------|
| DB Engine       | MySQL |
| DB Identifier   | prod-rds-finance |
| Environment     | Production |
| Department      | Finance |
| Classification | Confidential |

---

## 5. IAM Users and Attributes
IAM users were created with **no direct permissions**.  
All access decisions are evaluated at runtime using ABAC policies based on user and resource tags.

| User Name    | Department   | Seniority | Owner  |
|-------------|--------------|-----------|--------|
| eng-junior  | Engineering  | Junior    | Gibran |
| eng-senior  | Engineering  | Senior    | Gibran |
| fin-senior  | Finance      | Senior    | Gibran |
| hr-junior   | HR           | Junior    | Gibran |

> No IAM policies were attached directly to users.  
> Access is controlled entirely through ABAC policies.

---

## 6. ABAC Policy Design
A **single centralized IAM policy** was implemented to enforce ABAC across all AWS services.

The policy relies exclusively on:
- `aws:PrincipalTag`
- `aws:ResourceTag`

### Enforced Rules:
1. **Department Matching**  
   Users can access resources only when:
