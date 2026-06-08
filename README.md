# aws-iam-privilege-escalation-lab
# Overview

This project demonstrates a real-world cloud security vulnerability (IAM misconfiguration) and how it can lead to privilege escalation risks in AWS environments.

## The lab simulates:

- Creation of an IAM user
- Assignment of overly permissive access (AdministratorAccess)
- Verification of excessive privileges using AWS CLI
- Remediation using Least Privilege Principle
- Final validation of secure configuration
## Objective

The goal of this project is to:

- Understand IAM permission risks in AWS
- Simulate privilege escalation through misconfiguration
- Demonstrate real-world cloud security impact
- Apply the Principle of Least Privilege (PoLP)
- Strengthen AWS security hardening skills
#  Deployment / Setup Instructions
## Prerequisites
- AWS Account
- IAM user permissions (admin for setup)
- AWS CLI installed
- PowerShell or terminal configured
##  Step 1: Create IAM User
- Go to AWS Console → IAM
- I Created user: stephen-user
- Enable Programmatic access
- Download .csv credentials
## Step 2: Configure AWS CLI
```bash
aws configure
```

Enter:

- Access Key ID AKIAXQKXVVIJAAUPCJGU(from CSV)
- Secret Access Key
- Region: us-east-1
- Output: json


##  Step 3: Assign Misconfigured Policy

Attach AdministratorAccess:

```bash
aws iam attach-user-policy \
--user-name stephen-user \
--policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```
## Step 4: Verify Privileges
```bash
aws sts get-caller-identity
aws iam list-users
aws ec2 describe-instances
```
## Step 5: Fix (Least Privilege)

Remove admin access:
```bash
aws iam detach-user-policy \
--user-name stephen-user \
--policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```
Apply restricted access:
```bash
aws iam attach-user-policy \
--user-name stephen-user \
--policy-arn arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess
```
## Step 6: Verify Fix
```bash
aws iam list-users   # should fail
aws ec2 describe-instances   # should work
```
# Security Vulnerability

The IAM user was initially assigned AdministratorAccess, which:

- Grants full control over AWS resources
- Allows creation/deletion of IAM users
- Enables potential privilege escalation
- Violates the Principle of Least Privilege
# Fix Implemented

The vulnerability was mitigated by:

- Removing AdministratorAccess policy
- Applying EC2 ReadOnly Access
- Restricting permissions to minimal required access
# What I Learned
- IAM misconfigurations are one of the most critical cloud security risks
- Over-permissioned users can lead to full account compromise
- AWS CLI is powerful for security auditing and validation
- Least Privilege is essential in cloud security design
- Proper IAM design reduces attack surface significantly
# What I Would Improve
- Use IAM Roles instead of long-term access keys
- Enable AWS CloudTrail for audit logging
- Implement MFA for IAM users
- Use AWS IAM Access Analyzer for continuous monitoring
- Automate policy compliance checks using AWS Config
