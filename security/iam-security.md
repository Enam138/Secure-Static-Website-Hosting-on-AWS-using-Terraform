# IAM Security

## Introduction

AWS Identity and Access Management (IAM) is the primary service used to control authentication and authorization within an AWS environment. It enables organizations to securely manage identities and define what actions users, groups, and roles are permitted to perform.

Although the Akwannya Hub static website architecture relies heavily on managed AWS services, IAM remains a critical component for securing infrastructure deployment and ongoing administration.

This document describes how IAM principles were applied to protect the project's AWS resources and support secure operations.


# Objectives

The IAM strategy for this project was designed to:

- Restrict administrative access to authorized users.
- Apply the Principle of Least Privilege.
- Prevent unauthorized modifications to infrastructure.
- Secure Terraform deployments.
- Control access to AWS resources.
- Reduce the attack surface.


# IAM Overview

IAM provides three primary methods of access control.

| Component | Purpose |
|-----------|---------|
| Users | Individual identities that authenticate to AWS |
| Groups | Collections of users sharing common permissions |
| Roles | Temporary identities assumed by trusted entities or AWS services |

This project primarily relies on IAM users for administration and IAM service permissions managed by AWS.


# Principle of Least Privilege

The Principle of Least Privilege (PoLP) states that every identity should receive only the permissions required to perform its intended tasks.

Applying least privilege reduces the potential impact of:

- Accidental configuration changes
- Privilege escalation
- Compromised credentials
- Insider threats

Throughout this project, permissions were granted only where necessary.

# Administrative Access

Administrative tasks such as Terraform deployments and infrastructure management are performed using authenticated AWS credentials.

Examples include:

- Creating Amazon S3 buckets
- Deploying CloudFront distributions
- Managing Route 53 records
- Requesting ACM certificates
- Updating Terraform-managed resources

Administrative credentials should be protected using strong authentication mechanisms.


# Service Permissions

AWS managed services interact with one another through controlled permissions.

Examples include:

| Service | Required Access |
|----------|-----------------|
| CloudFront | Read objects from Amazon S3 |
| Amazon S3 | Allow requests only from the configured CloudFront distribution |
| ACM | Provide SSL/TLS certificates for CloudFront |
| Route 53 | Resolve the custom domain to the CloudFront distribution |

These permissions ensure that services communicate securely without exposing unnecessary access.


# Protecting AWS Credentials

AWS credentials should never be embedded directly in:

- Terraform files
- Source code
- Git repositories
- Application configuration files

Instead, credentials should be managed securely using the AWS CLI, environment variables, or other supported credential management methods.

The project's `.gitignore` file should exclude files that may contain sensitive information.


# Multi-Factor Authentication (MFA)

Although this project does not configure IAM directly through Terraform, administrative AWS accounts should have Multi-Factor Authentication (MFA) enabled.

Benefits include:

- Additional authentication factor
- Reduced risk of account compromise
- Protection against stolen passwords
- Improved account security


# Password Security

Strong passwords remain an important security control.

Recommended practices include:

- Long passwords or passphrases
- Unique passwords
- Password manager usage
- Regular review of credential security


# Terraform and IAM

Terraform requires permissions to provision AWS resources.

Rather than using the AWS account root user, deployments should be performed using an IAM identity with only the permissions necessary to create and manage the resources defined in the Terraform configuration.

This approach supports secure and auditable infrastructure management.


# Credential Rotation

Long-lived credentials should be rotated periodically.

Credential rotation helps reduce the impact of compromised access keys and supports ongoing operational security.


# IAM Best Practices

The project aligns with several IAM best practices:

- Use IAM identities instead of the AWS account root user.
- Enable Multi-Factor Authentication.
- Apply the Principle of Least Privilege.
- Protect AWS access keys.
- Avoid embedding credentials in source code.
- Rotate credentials regularly.
- Review permissions periodically.
- Manage infrastructure through Terraform.


# Security Benefits

Applying IAM best practices provides several benefits.

| Benefit | Description |
|----------|-------------|
| Access Control | Limits who can manage AWS resources |
| Reduced Attack Surface | Minimizes unnecessary permissions |
| Improved Accountability | Administrative actions can be traced |
| Credential Protection | Reduces exposure of AWS secrets |
| Secure Automation | Enables controlled Terraform deployments |


# Validation

The following IAM-related practices were reviewed during the project.

| Validation Item | Status |
|-----------------|--------|
| Least Privilege Applied | passed |
| Root Account Avoided | passed |
| Credentials Managed Securely | passed |
| No Secrets Stored in Repository | passed |
| Terraform Uses Authenticated AWS Identity | passed |


# Summary

IAM forms the foundation of access management within AWS and is essential to maintaining the security of the Akwannya Hub infrastructure.

By applying least-privilege principles, protecting administrative credentials, enabling Multi-Factor Authentication, and avoiding the use of the AWS account root user for routine tasks, the project follows AWS security best practices and promotes secure infrastructure management.

Although the architecture is relatively simple, these IAM practices help ensure that administrative access remains controlled, auditable, and resilient against common security risks.
