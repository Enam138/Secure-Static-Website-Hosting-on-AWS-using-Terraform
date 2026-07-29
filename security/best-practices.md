# AWS Security Best Practices

## Introduction

Building secure cloud infrastructure requires more than deploying AWS services. It involves applying proven security principles throughout the design, deployment, and operation of the environment.

The Akwannya Hub static website project incorporates AWS security best practices to protect website content, reduce operational risk, and support a secure, scalable, and maintainable architecture.

This document summarizes the key security practices adopted during the implementation of the project.

# Security Principles

The architecture was designed around several widely accepted security principles:

- Principle of Least Privilege
- Defense in Depth
- Secure by Default
- Encryption Everywhere
- Infrastructure as Code
- Continuous Validation
- Managed Security Services

These principles guided both the architecture and deployment decisions.

# Principle of Least Privilege

Least Privilege ensures that identities and services receive only the permissions necessary to perform their intended tasks.

### Implementation

- Administrative access is limited to authorized IAM identities.
- CloudFront is granted read access only to the S3 bucket.
- The S3 bucket accepts requests only from the configured CloudFront distribution.
- The AWS account root user is not used for routine administration.

### Benefits

- Reduced attack surface
- Lower risk of privilege escalation
- Improved security governance

# Defense in Depth

Rather than relying on a single security mechanism, the architecture uses multiple layers of protection.

### Security Layers

| Layer | AWS Service |
|--------|-------------|
| DNS | Amazon Route 53 |
| Transport Security | HTTPS / TLS |
| Certificate Management | AWS Certificate Manager |
| Content Delivery | Amazon CloudFront |
| Origin Protection | Origin Access Control |
| Storage | Amazon S3 |
| Access Control | Bucket Policy |
| Data Protection | Server-Side Encryption |
| Identity | AWS IAM |
| Infrastructure | Terraform |

Each layer provides an additional level of protection should another control fail.

# Secure by Default

Where possible, secure default configurations were selected.

### Examples

- Amazon S3 Block Public Access enabled
- Private S3 bucket
- HTTPS enforced
- Server-side encryption enabled
- Origin Access Control enabled
- Versioning enabled

These defaults reduce the likelihood of accidental exposure.

# Encryption in Transit

Website traffic is encrypted between users and AWS.

### Implementation

- HTTPS enforced
- AWS Certificate Manager
- TLS
- CloudFront Viewer Certificate

### Benefits

- Protects data confidentiality
- Prevents traffic interception
- Builds user trust

# Encryption at Rest

Website assets stored in Amazon S3 are protected using Server-Side Encryption (SSE-S3).

### Benefits

- Automatic encryption
- AWS-managed keys
- Transparent operation
- Minimal administrative overhead

# Protecting the Origin

A key architectural decision was preventing direct public access to the website's storage.

### Implementation

- Private Amazon S3 bucket
- Origin Access Control
- Bucket Policy
- Block Public Access

Website visitors interact only with CloudFront.

# Infrastructure as Code

Terraform is used to provision all AWS resources.

### Benefits

- Consistent deployments
- Version-controlled infrastructure
- Reduced configuration drift
- Easier auditing
- Repeatable deployments

Managing infrastructure as code improves reliability and operational efficiency.

# Managed AWS Services

Where possible, AWS-managed services were preferred over self-managed alternatives.

Examples include:

- Amazon CloudFront
- Amazon Route 53
- AWS Certificate Manager
- Amazon S3

Using managed services reduces administrative overhead while benefiting from AWS-managed availability, scalability, and security updates.

# Secure DNS Management

Amazon Route 53 manages DNS records for the custom domain.

Benefits include:

- Highly available DNS
- Reliable domain resolution
- Integration with CloudFront
- Support for ACM DNS validation

# Versioning

Amazon S3 Versioning protects against accidental deletion or modification of website assets.

Benefits include:

- Object recovery
- Rollback capability
- Improved resilience
- Simplified disaster recovery

# Secure Certificate Management

AWS Certificate Manager automates certificate lifecycle management.

Benefits include:

- Automatic renewal
- Secure storage
- DNS validation
- Integration with CloudFront

This eliminates the operational burden of manually managing SSL/TLS certificates.

# Validation and Testing

Security controls were verified after deployment.

Validation activities included:

- HTTPS verification
- CloudFront deployment verification
- Origin Access Control validation
- Bucket security review
- Encryption verification
- DNS validation
- Website accessibility testing

# Alignment with AWS Well-Architected Framework

The project incorporates practices that support several pillars of the AWS Well-Architected Framework.

| Pillar | Implementation |
|---------|----------------|
| Security | Encryption, IAM, OAC, Private S3 |
| Reliability | CloudFront, Route 53, Versioning |
| Performance Efficiency | Global CloudFront edge locations |
| Cost Optimization | Managed services and static website architecture |
| Operational Excellence | Terraform, documentation, validation |

# Security Checklist

The following controls were implemented during the project.

| Control | Status |
|----------|--------|
| Private Amazon S3 bucket | Passed |
| Block Public Access | Passed |
| HTTPS enforced | Passed |
| TLS enabled | Passed |
| AWS Certificate Manager | Passed |
| Origin Access Control | Passed |
| Bucket Policy | Passed |
| Server-Side Encryption | Passed |
| Versioning | Passed |
| Least Privilege | Passed |
| Infrastructure as Code | Passed |
| Terraform Validation | Passed |


# Continuous Improvement

Security is an ongoing process rather than a one-time activity.

Future enhancements for this architecture could include:

- AWS WAF for application-layer protection
- CloudFront security headers
- Amazon GuardDuty for threat detection
- AWS Config for compliance monitoring
- AWS CloudTrail for audit logging
- Amazon CloudWatch alarms
- Security Hub for centralized security posture management

These services would further strengthen the security posture as the environment grows.

# Summary

The Akwannya Hub static website architecture incorporates AWS security best practices across identity management, network security, encryption, access control, and infrastructure management.

By combining managed AWS services, Infrastructure as Code, encryption, Origin Access Control, private storage, and least-privilege access, the solution delivers a secure and maintainable platform for hosting static web content while aligning with AWS architectural guidance and industry-recognized security principles.
