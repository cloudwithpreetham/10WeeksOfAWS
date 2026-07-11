# Day 1 - AWS Cloud Foundations

## Goal

Understand how AWS is organized, learn the AWS Global Infrastructure, and understand the AWS Shared Responsibility Model.

---

# Before You Start

Write these answers in your own words before logging into AWS.

### 1. What is a Region?

> A Region is a physical geographic location where AWS hosts multiple data centers to provide cloud services.

---

### 2. What is an Availability Zone?

> An Availability Zone (AZ) is one or more isolated data centers inside an AWS Region that provide high availability and fault tolerance.

---

### 3. What is one thing AWS secures?

> AWS secures the physical infrastructure, including data centers, networking equipment, and hardware.

---

### 4. What is one thing you must secure?

> I am responsible for securing my applications, IAM users, permissions, data, operating systems, and network configurations.

---

# AWS Global Infrastructure

AWS infrastructure is built using three main components.

## 1. Region

A Region is a geographic area where AWS provides cloud services.

Examples:

- us-east-1 (N. Virginia)
- us-west-2 (Oregon)
- ap-south-1 (Mumbai)

Each Region is completely independent from the others.

---

## 2. Availability Zone (AZ)

An Availability Zone consists of one or more isolated data centers inside a Region.

Benefits:

- High Availability
- Fault Tolerance
- Disaster Recovery

A Region usually contains multiple Availability Zones.

Example:

```
Mumbai Region (ap-south-1)

├── AZ-a
├── AZ-b
└── AZ-c
```

Deploying resources across multiple AZs improves application availability.

---

## 3. Edge Location

Edge Locations are global locations used by Amazon CloudFront and other AWS services to deliver content closer to users.

Benefits:

- Lower latency
- Faster content delivery
- Better user experience

---

# AWS Global Infrastructure Diagram

```
                AWS Global Infrastructure

                  Region (Mumbai)

        ┌─────────────────────────────┐
        │                             │
        │  Availability Zone A        │
        │                             │
        ├─────────────────────────────┤
        │                             │
        │  Availability Zone B        │
        │                             │
        ├─────────────────────────────┤
        │                             │
        │  Availability Zone C        │
        │                             │
        └─────────────────────────────┘

               ↑
               │

      Edge Locations (Worldwide)

Users → Edge Location → AWS Region
```

---

# AWS Shared Responsibility Model

AWS security responsibilities are shared between AWS and the customer.

## AWS is Responsible For

(Security **of** the Cloud)

- Physical Data Centers
- Hardware
- Networking Infrastructure
- Storage Infrastructure
- Global AWS Infrastructure
- Managed Service Infrastructure

---

## Customer is Responsible For

(Security **in** the Cloud)

- IAM Users
- IAM Roles
- Passwords
- MFA
- Applications
- Data
- Security Groups
- Network ACLs
- EC2 Operating System Updates
- Application Patching

---

# Shared Responsibility Table

| AWS Manages        | Customer Manages    |
| ------------------ | ------------------- |
| Physical Security  | IAM Users           |
| Data Centers       | IAM Roles           |
| Servers            | Applications        |
| Networking         | Data                |
| Storage Hardware   | Security Groups     |
| AWS Infrastructure | EC2 OS Updates      |
| Managed Services   | Application Patches |

---

# Easy Way to Remember

> **AWS secures the cloud. You secure what you build in the cloud.**

---

# AWS Root User

The Root User is the owner of the AWS account and has unrestricted access to all AWS services and resources.

Use the Root User only for:

- Initial AWS account setup
- Billing management
- MFA configuration
- Account recovery

Avoid using the Root User for daily work.

---

# Root User Best Practices

- Enable Multi-Factor Authentication (MFA)
- Never share Root credentials
- Create an IAM User for daily work
- Use least privilege access
- Monitor AWS billing regularly

---

# Exam Tips

✔ A Region contains multiple Availability Zones.

✔ Availability Zones provide High Availability.

✔ CloudFront uses Edge Locations.

✔ AWS follows the Shared Responsibility Model.

✔ The Root User should not be used for everyday tasks.

---

# Today's Learning Summary

Today I learned:

- What an AWS Region is.
- What an Availability Zone is.
- What an Edge Location is.
- How AWS Global Infrastructure is organized.
- The AWS Shared Responsibility Model.
- The difference between security **of** the cloud and security **in** the cloud.
- Root User best practices.
- Why deploying resources across multiple Availability Zones improves availability.

---

# Interview Questions

### Q1. What is an AWS Region?

A geographic area where AWS hosts multiple Availability Zones.

---

### Q2. What is an Availability Zone?

An isolated data center (or group of data centers) within a Region that provides fault tolerance.

---

### Q3. What is an Edge Location?

A location used by AWS services like CloudFront to deliver content closer to users with lower latency.

---

### Q4. What is the AWS Shared Responsibility Model?

A security model where AWS secures the cloud infrastructure, while customers secure their applications, identities, and data.

---

### Q5. Why should you avoid using the Root User?

Because it has unrestricted permissions and should only be used for account-level administrative tasks.

---

# Key Takeaways

- Region = Geographic location.
- Availability Zone = Isolated data center.
- Edge Location = Faster content delivery.
- AWS secures the cloud.
- Customers secure their workloads in the cloud.
- Enable MFA on the Root User.
- Use IAM Users and Roles for daily operations.
