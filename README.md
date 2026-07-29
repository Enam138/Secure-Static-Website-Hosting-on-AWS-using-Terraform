# Akwannya Hub Static Website Infrastructure on AWS using Terraform

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazonaws)
![Terraform](https://img.shields.io/badge/Terraform-Infrastructure_as_Code-623CE4?logo=terraform)
![Amazon S3](https://img.shields.io/badge/Amazon_S3-Storage-green?logo=amazons3)
![CloudFront](https://img.shields.io/badge/Amazon_CloudFront-CDN-blue)
![Route53](https://img.shields.io/badge/Amazon_Route_53-DNS-red)
![ACM](https://img.shields.io/badge/AWS_Certificate_Manager-SSL-yellow)
![License](https://img.shields.io/badge/License-MIT-green)

A production-style cloud infrastructure project demonstrating how to securely host a static website on **Amazon Web Services (AWS)** using **Terraform**. The project implements Infrastructure as Code (IaC), secure content delivery, private storage, HTTPS, DNS management, and comprehensive technical documentation.

#  Table of Contents

- [Project Overview](#project-overview)
- [Architecture](#architecture)
- [Project Highlights](#project-highlights)
- [Business Scenario](#business-scenario)
- [Solution Architecture](#solution-architecture)
- [AWS Services Used](#aws-services-used)
- [Repository Structure](#repository-structure)
- [Features](#features)
- [Security Highlights](#security-highlights)
- [Deployment Overview](#deployment-overview)
- [Validation](#validation)
- [Documentation](#documentation)
- [Screenshots](#screenshots)
- [Skills Demonstrated](#skills-demonstrated)
- [Future Improvements](#future-improvements)
- [Lessons Learned](#lessons-learned)
- [References](#references)
- [Author](#author)
- [License](#license)


# Project Overview

This project demonstrates how to design, deploy, and secure a **cloud-native static website architecture** using AWS managed services and Terraform.

The infrastructure emphasizes:

- Infrastructure as Code (Terraform)
- Secure static website hosting
- Private Amazon S3 origin
- Global content delivery using CloudFront
- HTTPS with AWS Certificate Manager
- Custom domain using Route 53
- Security best practices
- Production-ready documentation

Rather than manually configuring AWS resources, the entire infrastructure is defined as code, making deployments repeatable, consistent, and easier to maintain.

# Architecture

> **Architecture Diagram**

<img width="1536" height="1024" alt="akwannya" src="https://github.com/user-attachments/assets/a63eb2a1-4e9d-4b9a-95fd-9f0a820af41d" />


The solution routes user requests through Amazon CloudFront before securely accessing content stored in a private Amazon S3 bucket.

```
Users
   │
   ▼
Amazon Route 53
   │
   ▼
Amazon CloudFront
   │
 HTTPS (ACM)
   │
Origin Access Control
   │
   ▼
Private Amazon S3 Bucket
```

# Project Highlights

 Infrastructure as Code using Terraform

 Private Amazon S3 bucket

 Amazon CloudFront CDN

 Origin Access Control (OAC)

 HTTPS with AWS Certificate Manager

 Amazon Route 53 DNS

 Server-Side Encryption

 Amazon S3 Versioning

 Block Public Access

 Modular Terraform Architecture

 Comprehensive Documentation


# Business Scenario

Akwannya Hub required a secure, scalable, and cost-effective platform for hosting its public website.

The solution needed to:

- Deliver website content globally with low latency.
- Secure the origin storage from public access.
- Support HTTPS using a trusted SSL/TLS certificate.
- Simplify infrastructure management through automation.
- Follow AWS security and architectural best practices.

# Solution Architecture

The architecture combines AWS managed services to deliver a secure and highly available solution.

| Layer | AWS Service |
|---------|-------------|
| DNS | Amazon Route 53 |
| SSL/TLS | AWS Certificate Manager |
| CDN | Amazon CloudFront |
| Origin Protection | Origin Access Control |
| Storage | Amazon S3 |
| Infrastructure | Terraform |

This architecture minimizes operational overhead while providing strong security and scalability.

# AWS Services Used

| Service | Purpose |
|---------|---------|
| Amazon S3 | Static website storage |
| Amazon CloudFront | Global content delivery |
| Amazon Route 53 | DNS management |
| AWS Certificate Manager | SSL/TLS certificates |
| AWS IAM | Access management |
| Terraform | Infrastructure as Code |

# Repository Structure

```
.
├── architecture/
├── deployment/
├── security/
├── monitoring/
├── troubleshooting/
├── diagrams/
├── screenshots/
├── modules/
├── README.md
├── lessons-learned.md
└── LICENSE
```


# Features

- Secure static website hosting
- Infrastructure as Code
- HTTPS enabled
- Private S3 origin
- CloudFront content delivery
- Custom domain support
- DNS management
- Server-side encryption
- Versioning
- Modular Terraform configuration
- Detailed documentation

# Security Highlights

The project incorporates multiple AWS security controls.

- Private Amazon S3 bucket
- Origin Access Control (OAC)
- Block Public Access
- HTTPS enforcement
- TLS encryption
- AWS Certificate Manager
- Bucket Policies
- Server-Side Encryption (SSE-S3)
- Principle of Least Privilege
- Infrastructure as Code

A complete security assessment is available in the **security/** documentation.

# Deployment Overview

Deployment is performed entirely using Terraform.

```bash
terraform init

terraform validate

terraform plan

terraform apply
```

The deployment provisions:

- Amazon S3
- Amazon CloudFront
- Route 53
- ACM Certificate
- Origin Access Control
- Bucket Policies

# Validation

The deployed infrastructure was validated to confirm:

-  HTTPS enabled
-  CloudFront operational
-  DNS resolution successful
-  Private S3 bucket
-  Origin Access Control functioning
-  Server-side encryption enabled
-  Versioning enabled
-  Website accessible

Detailed validation results are available in:

`deployment/validation.md`


# Documentation

## Architecture

- [Architecture Overview](architecture/architecture-overview.md)
- [AWS Services](architecture/aws-services.md)
- [Request Flow](architecture/request-flow.md)
- [Terraform Modules](architecture/terraform-modules.md)

## Deployment

- [Prerequisites](deployment/prerequisites.md)
- [Deployment Guide](deployment/deployment-guide.md)
- [Deployment Validation](deployment/validation.md)

## Security

- [Security Controls](security/security-controls.md)
- [Encryption](security/encryption.md)
- [IAM Security](security/iam-security.md)
- [Threat Model](security/threat-model.md)
- [AWS Security Best Practices](security/best-practices.md)

## Monitoring

- [Monitoring](monitoring/monitoring.md)

## Troubleshooting

- [Common Issues](troubleshooting/common-issues.md)


# Screenshots

Add screenshots here after deployment.

### AWS Architecture

```
screenshots/architecture.png
```

### Amazon CloudFront

```
screenshots/cloudfront.png
```

### Amazon S3

```
screenshots/s3.png
```

### Route 53

```
screenshots/route53.png
```

### AWS Certificate Manager

```
screenshots/acm.png
```

### Terraform Deployment

```
screenshots/terraform-apply.png
```

### Live Website

```
screenshots/website-homepage.png
```


# Skills Demonstrated

This project demonstrates practical experience with:

- AWS Cloud Architecture
- Terraform
- Infrastructure as Code
- Cloud Security
- DNS Management
- Content Delivery Networks
- HTTPS / TLS
- Amazon S3
- CloudFront
- Route 53
- IAM Best Practices
- Technical Documentation
- Cloud Operations
- Solution Architecture


# Future Improvements

Potential future enhancements include:

- AWS WAF
- AWS CloudTrail
- Amazon GuardDuty
- AWS Config
- AWS Security Hub
- CloudWatch Alarms
- GitHub Actions CI/CD Pipeline
- Automated Terraform Testing


# Lessons Learned

Key takeaways from this project include:

- Infrastructure as Code improves consistency and maintainability.
- Security should be built into the architecture from the beginning.
- Managed AWS services reduce operational overhead.
- Comprehensive documentation is an important project deliverable.
- Monitoring and validation are essential for production-ready infrastructure.

A detailed reflection is available in:

`lessons-learned.md`


# References

- AWS Documentation
- Terraform Documentation
- AWS Well-Architected Framework
- AWS Security Best Practices


# Author

**Manyo Sampson**

Cloud Security Engineer| SOC Analyst | AWS & Azure 

Feel free to connect or explore the repository to learn more about the implementation.
