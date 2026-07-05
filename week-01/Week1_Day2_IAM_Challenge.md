# Week 1 Day 2 Challenge — Cloud Foundations + IAM

**AWS Zero To Hero | CloudAdhar × TrainWithShubham**  
**Module:** Week 1 — Cloud Foundations + IAM  
**Focus:** IAM Users, Groups, Roles, Policies, JSON Policy, Least Privilege, Permission Boundaries  
**Exam Alignment:** SAA-C03 Domain 1 — Design Secure Architectures — 30%  
**Primary Well-Architected Pillar:** Security  
**Secondary Pillars:** Operational Excellence, Cost Optimization  

---

## Challenge Goal

Today’s challenge connects **Day 1: Cloud Foundations** and **Day 2: IAM Fundamentals**.

By the end of this challenge, you should be able to:

- Secure your AWS account using MFA and billing alerts
- Explain why the root user should not be used for daily work
- Create IAM users and IAM groups
- Attach AWS managed policies to groups
- Understand managed policies vs customer managed policies vs inline policies
- Read and write a basic JSON IAM policy
- Apply the principle of least privilege
- Understand IAM roles for AWS services like EC2 accessing S3
- Understand permission boundaries at a high level

---

## Day 1 Revision Topics

Before starting today’s IAM challenge, revise these Day 1 concepts:

- AWS Global Infrastructure
- AWS Regions and Availability Zones
- Shared Responsibility Model
- AWS account setup
- Root user security
- MFA
- Billing dashboard
- Billing alerts / budget alerts

---

## Day 2 Topics

Today’s practical focus:

- IAM users, groups, and roles
- IAM policies
- AWS managed policy
- Customer managed policy
- Inline policy
- JSON policy structure
- Principle of least privilege
- Permission boundaries
- IAM role for EC2 to access S3
- S3 access, EC2 access, and billing access

---

## Important Security Note

Do **not** share the following in screenshots or commits:

- AWS account ID
- Root email address
- Access keys
- Secret access keys
- MFA QR code
- Billing details
- Payment details
- Any confidential account information

Mask sensitive information before uploading screenshots.

---

# Part 1 — Secure Your AWS Account

## Task 1: Verify Root MFA and Billing Alert

Complete these actions in your AWS account:

- Enable MFA on the root user
- Open the AWS Billing Dashboard
- Create or verify a billing alert / budget alert
- Confirm you are not using the root user for daily practice

## Deliverables

Add screenshots for:

- Root MFA enabled
- Billing dashboard or billing alert
- Budget / alarm configuration

---

# Part 2 — IAM Group for S3 Read-Only Access

## Task 2: Create S3 Read-Only Group

Create an IAM group:

```text
Group name: S3ReadOnlyGroup
Policy: AmazonS3ReadOnlyAccess
```

Create an IAM user:

```text
User name: learner-s3
Add user to: S3ReadOnlyGroup
```

## Test

Login as `learner-s3` and verify:

- You can open the S3 console
- You can list/view S3 buckets
- You cannot perform actions outside the attached permission scope

## Deliverables

Add screenshots for:

- IAM group created
- IAM user created
- User added to group
- Policy attached to group
- S3 access test
- Access denied result, if tested

---

# Part 3 — IAM Group for EC2 Read-Only Access

## Task 3: Create EC2 Read-Only Group

Create an IAM group:

```text
Group name: EC2ReadOnlyGroup
Policy: AmazonEC2ReadOnlyAccess
```

Create an IAM user:

```text
User name: learner-ec2
Add user to: EC2ReadOnlyGroup
```

## Test

Login as `learner-ec2` and verify:

- You can open the EC2 dashboard
- You can view EC2 resources
- You cannot create, start, stop, or terminate EC2 instances

## Deliverables

Add screenshots for:

- EC2ReadOnlyGroup
- Policy attached
- learner-ec2 user added
- EC2 dashboard visible
- Denied action, if tested

---

# Part 4 — IAM Group for Billing View Access

## Task 4: Create Billing View Group

Create an IAM group:

```text
Group name: BillingViewGroup
Policy: AWSBillingReadOnlyAccess
```

Create an IAM user:

```text
User name: learner-billing
Add user to: BillingViewGroup
```

## Test

Login as `learner-billing` and verify:

- You can open the Billing Dashboard
- You can view billing information
- You cannot manage unrelated AWS services

## Deliverables

Add screenshots for:

- BillingViewGroup
- Billing policy attached
- learner-billing user added
- Billing Dashboard access

---

# Part 5 — Custom JSON IAM Policy

## Task 5: Create a Custom S3 Read-Only Policy

Create a customer managed policy:

```text
Policy name: CustomS3ReadOnlyTrainingPolicy
```

Use the following JSON policy and replace `YOUR-BUCKET-NAME` with your actual S3 bucket name.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ListAllBuckets",
      "Effect": "Allow",
      "Action": [
        "s3:ListAllMyBuckets",
        "s3:GetBucketLocation"
      ],
      "Resource": "*"
    },
    {
      "Sid": "ListSpecificBucket",
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket"
      ],
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME"
    },
    {
      "Sid": "ReadObjectsFromSpecificBucket",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
    }
  ]
}
```

## Deliverables

Add:

- JSON policy file in your repo
- Screenshot of customer managed policy created
- Screenshot of allowed action
- Screenshot of denied action, if tested

---

# Part 6 — IAM Role for EC2 to Access S3

## Task 6: Create EC2 Role for S3 Read-Only Access

Create an IAM role:

```text
Role name: EC2S3ReadOnlyRole
Trusted entity: AWS Service
Use case: EC2
Policy: AmazonS3ReadOnlyAccess
```

## Key Learning

This is the secure approach:

```text
EC2 instance → IAM Role → Temporary credentials → S3 access
```

Avoid this approach:

```text
EC2 instance → Hardcoded access keys → S3 access
```

## Deliverables

Add screenshots for:

- Role created
- Trusted entity as EC2
- S3 read-only policy attached
- Role summary page

---

# Optional Advanced Task — Switch Role

This is optional for learners who want to go deeper.

## Task 7: Create a Switch Role Demo

Create an IAM role:

```text
Role name: TrainingReadOnlyRole
Policy: ReadOnlyAccess
```

Create a policy that allows an IAM user to assume this role:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAssumeTrainingReadOnlyRole",
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "arn:aws:iam::ACCOUNT-ID:role/TrainingReadOnlyRole"
    }
  ]
}
```

Replace:

```text
ACCOUNT-ID
```

with your AWS account ID.

## Test

- Login as an IAM user
- Switch role from AWS Console
- Verify role-based access

## Key Learning

```text
IAM User = login identity
IAM Role = temporary access
STS = provides temporary credentials
Policy = defines allowed or denied actions
```

---

# Optional Advanced Task — Permission Boundary

## Task 8: Understand Permission Boundary

A permission boundary defines the **maximum permissions** an IAM user or role can ever receive.

Example understanding:

```text
Attached policy may allow S3 and EC2.
Permission boundary may allow only S3.
Effective access = only S3.
```

## Key Learning

```text
Policy defines what is allowed.
Permission boundary defines the maximum limit.
```

This is useful when permission management is delegated, but you still want to control the maximum possible access.

---

# Repository Submission Format

Create your folder like this:

```text
week-01/day-02-iam/YOUR-NAME/
```

Inside your folder, add:

```text
README.md
notes.md
policies/
screenshots/
```

Recommended structure:

```text
week-01/
└── day-02-iam/
    └── your-name/
        ├── README.md
        ├── notes.md
        ├── policies/
        │   └── custom-s3-readonly-policy.json
        └── screenshots/
            ├── root-mfa.png
            ├── billing-alert.png
            ├── iam-group-s3.png
            ├── iam-user-s3.png
            ├── s3-access-test.png
            ├── ec2-readonly-group.png
            ├── billing-view-group.png
            └── iam-role-ec2-s3.png
```

---

# README.md Template for Your Submission

Use this template inside your folder:

```markdown
# Week 1 Day 2 — IAM Challenge

## Name

Your Name

## Topics Practiced

- AWS account security
- Root MFA
- Billing alert
- IAM users
- IAM groups
- IAM roles
- IAM policies
- JSON policy
- Least privilege
- Permission boundaries
- EC2 role for S3 access

## What I Learned

Today I learned how IAM controls access in AWS.

The most important concept I understood is:

> Identity + Permissions = Access

I also learned that the root user should not be used for daily work and IAM roles are better for AWS services like EC2 accessing S3.

## Hands-on Completed

- [ ] Verified root MFA
- [ ] Verified billing alert
- [ ] Created S3ReadOnlyGroup
- [ ] Created learner-s3 IAM user
- [ ] Attached AmazonS3ReadOnlyAccess
- [ ] Tested S3 read-only access
- [ ] Created EC2ReadOnlyGroup
- [ ] Created BillingViewGroup
- [ ] Created custom S3 read-only JSON policy
- [ ] Created EC2S3ReadOnlyRole
- [ ] Understood permission boundaries

## Screenshots Added

- Root MFA
- Billing alert
- IAM user
- IAM group
- Policy attached
- S3 access test
- EC2 read-only group
- Billing view group
- IAM role

## Custom Policy

Policy file added:

```text
policies/custom-s3-readonly-policy.json
```

## Key Takeaway

Least privilege means giving only the required permissions, nothing extra.
```

---

# Cleanup

After completing the lab:

- Delete test IAM users
- Delete test IAM groups
- Delete custom test policies if not needed
- Keep billing alerts enabled
- Keep MFA enabled
- Do not delete your main admin IAM user
- Do not delete anything you are unsure about

---

# Practice Questions

## Q1. Where should you attach IAM policies for easiest management at scale?

A. Directly to every user  
B. To IAM groups  
C. To the root user  
D. To access keys  

**Answer:** B

---

## Q2. An EC2 instance needs to read from S3. What is the best secure approach?

A. Store access keys inside the EC2 instance  
B. Attach an IAM role to the EC2 instance  
C. Use root user credentials  
D. Make the bucket public  

**Answer:** B

---

## Q3. A user has a policy that allows only `s3:GetObject`. What can the user do?

A. Delete S3 buckets  
B. Read objects only  
C. Create IAM users  
D. Access everything in AWS  

**Answer:** B

---

## Q4. What does a permission boundary define?

A. Password policy  
B. Maximum permissions possible  
C. MFA device type  
D. Billing limit  

**Answer:** B

---

## Q5. What is the core idea of least privilege?

A. Give all users admin access  
B. Give only the required permissions  
C. Share one user for all team members  
D. Use root user for daily work  

**Answer:** B

---

# Learn in Public Task

Post your Week 1 Day 2 learning update on LinkedIn.

Mention:

- What you learned about IAM
- Why root user should not be used daily
- What you understood about users, groups, roles, and policies
- One screenshot from your practice without sensitive information

Tag:

- Gangadhar Ure
- Shubham Londhe

Mention:

```text
CloudAdhar × TrainWithShubham
```

Use hashtags:

```text
#10WeeksOfAWS
#AWS10WeekChallenge
#AWS
#IAM
#AWSSAA
#SAAC03
#CloudAdhar
#TrainWithShubham
```

---

# Sample LinkedIn Post

```text
Week 1 Day 2 of #10WeeksOfAWS completed.

Today I practiced AWS IAM fundamentals:

- IAM users, groups, and roles
- Managed policies and custom policies
- JSON policy structure
- Principle of least privilege
- EC2 role for S3 access
- Billing and account security basics

My key takeaway:

In AWS, login alone is not access.
Access is controlled by IAM permissions.

Identity + Permissions = Access.

Thank you Gangadhar Ure and Shubham Londhe for the session.

CloudAdhar × TrainWithShubham

#10WeeksOfAWS #AWS10WeekChallenge #AWS #IAM #AWSSAA #SAAC03 #CloudAdhar #TrainWithShubham
```

---

# Final Reminder

IAM is not only a service.  
IAM is the foundation of secure AWS architecture.

If IAM is weak, everything inside AWS becomes risky.

Focus on this formula:

```text
Identity + Permissions = Access
```
