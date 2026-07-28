# Secure Static Website Hosting on AWS with Terraform

![AWS](https://img.shields.io/badge/AWS-Cloud-orange)
![Terraform](https://img.shields.io/badge/IaC-Terraform-purple)
![CloudFront](https://img.shields.io/badge/CDN-CloudFront-blue)
![Amazon S3](https://img.shields.io/badge/Storage-Amazon_S3-green)
![Route53](https://img.shields.io/badge/DNS-Route_53-red)
![License](https://img.shields.io/badge/License-MIT-green)

## Project Overview

This project demonstrates the deployment of a secure, scalable, and production-ready static website on Amazon Web Services (AWS) using **Terraform** as the Infrastructure as Code (IaC) tool.

The architecture follows AWS Well-Architected Framework principles by leveraging fully managed AWS services to deliver highly available, globally distributed, and secure content without requiring traditional web servers.

The solution uses Amazon S3 as the origin for static website content, Amazon CloudFront as a global Content Delivery Network (CDN), Amazon Route 53 for DNS resolution, and AWS Certificate Manager (ACM) for SSL/TLS certificate management. Terraform provisions every cloud resource, ensuring consistent, repeatable, and automated deployments.

By combining Infrastructure as Code with AWS managed services, the solution provides a cost-effective hosting platform capable of serving static websites, portfolios, blogs, documentation sites, and landing pages while implementing modern cloud security best practices.

# Business Scenario

Akwannya Hub is a community-driven technology platform that connects students and professionals with scholarships, free technology training, career opportunities, and industry events. As the community continues to grow, the organization requires a secure, scalable, and highly available web presence capable of serving users from different geographic locations while maintaining strong security and performance.

To support these requirements, a cloud-native static website architecture was designed and deployed on Amazon Web Services (AWS). The solution leverages fully managed services to eliminate server administration while providing global content delivery, HTTPS encryption, automatic scalability, and low operational costs.

The website is hosted using Amazon S3 as the storage layer, Amazon CloudFront as the global Content Delivery Network (CDN), Amazon Route 53 for DNS management, and AWS Certificate Manager (ACM) for SSL/TLS certificate provisioning. Infrastructure provisioning and configuration are automated using Terraform, enabling repeatable deployments and Infrastructure as Code (IaC) best practices.

The resulting architecture delivers a secure, production-ready platform that supports Akwannya Hub's mission of making technology resources and opportunities accessible to a broad audience while following AWS Well-Architected Framework principles and cloud security best practices.

The organization wanted an architecture that could:

- Deliver website content globally with low latency.
- Secure traffic using HTTPS.
- Prevent unauthorized access to website storage.
- Support Infrastructure as Code.
- Scale automatically without manual intervention.
- Reduce operational costs.
- Follow AWS security best practices.

# Project Objectives

The primary objectives of this project were to:

- Deploy a static website using Amazon S3.
- Configure a custom domain using Amazon Route 53.
- Secure the website with HTTPS using AWS Certificate Manager.
- Improve website performance through Amazon CloudFront caching.
- Restrict direct access to the S3 bucket using Origin Access Control (OAC).
- Implement Infrastructure as Code using Terraform.
- Build reusable Terraform modules.
- Follow AWS Well-Architected Framework recommendations.
- Design a secure and production-ready cloud architecture.

# Solution Architecture

The architecture consists of several managed AWS services working together to provide secure website hosting.

```
Website Visitors
        │
        ▼
 Amazon Route 53
        │
        ▼
 Amazon CloudFront
        │
        ▼
 Origin Access Control (OAC)
        │
        ▼
 Private Amazon S3 Bucket
```

Request Flow

1. A user enters the website URL in a browser.

2. Amazon Route 53 resolves the domain name.

3. DNS directs the request to Amazon CloudFront.

4. CloudFront checks whether the requested object exists in its edge cache.

5. If cached, CloudFront immediately returns the content.

6. If not cached, CloudFront securely retrieves the object from the private S3 bucket.

7. The requested content is delivered to the user over HTTPS.

# AWS Services Used

| AWS Service | Purpose |
|-------------|----------|
| Amazon S3 | Stores static website assets |
| Amazon CloudFront | Global content delivery network |
| Amazon Route 53 | DNS management |
| AWS Certificate Manager | SSL/TLS certificate management |
| Terraform | Infrastructure provisioning |
| IAM | Permissions and access control |

# Architecture Highlights

The solution implements a modern serverless architecture with the following characteristics:

- Fully managed infrastructure
- No web servers
- No operating system maintenance
- Global content delivery
- Automatic scaling
- HTTPS by default
- Secure private storage
- Immutable Infrastructure
- Infrastructure as Code
- Modular Terraform design

# Security Features

Security was a key consideration throughout the design and implementation of this solution.

The following controls were implemented:

- HTTPS enforced across the website.
- TLS 1.2 encryption.
- AWS Certificate Manager SSL certificate.
- Private Amazon S3 bucket.
- Block Public Access enabled.
- CloudFront Origin Access Control (OAC).
- Bucket policy restricting access to CloudFront only.
- Server-side encryption (SSE-S3).
- Object versioning enabled.
- Least privilege access model.
- Minimal origin exposure.
- Immutable infrastructure deployments.

# High Availability and Scalability

The architecture leverages AWS managed services that automatically scale based on demand.

Availability features include:

- Amazon CloudFront global edge network.
- Amazon S3 eleven nines (99.999999999%) durability.
- Route 53 Anycast DNS.
- Automatic scaling.
- No infrastructure management.
- Zero-downtime deployments using immutable infrastructure.

# Terraform Implementation

Infrastructure was provisioned using reusable Terraform modules.

Modules include:

- Route 53 Module
- ACM Module
- CloudFront Module
- S3 Website Module

Terraform features implemented:

- Variables
- Outputs
- Data Sources
- Modular Architecture
- Dynamic Lookups
- Validation
- Strong Typing
- Resource Tagging

# Repository Structure

```
aws-static-website-terraform/
│
├── architecture/
├── deployment/
├── security/
├── monitoring/
├── troubleshooting/
├── diagrams/
├── screenshots/
└── README.md
```

# Skills Demonstrated

This project demonstrates practical experience with:

## Cloud Architecture

- AWS Solution Design
- Static Website Hosting
- CDN Architecture
- DNS Management
- HTTPS Implementation

## Infrastructure as Code

- Terraform
- Modular Infrastructure
- Reusable Modules
- State Management
- Variables and Outputs

## AWS Services

- Amazon S3
- Amazon CloudFront
- Amazon Route 53
- AWS Certificate Manager
- IAM

## Security

- HTTPS
- TLS
- Origin Access Control
- Server-side Encryption
- Least Privilege
- Block Public Access
- Secure Bucket Policies

# Best Practices Implemented

The deployment follows several AWS best practices, including:

- Infrastructure as Code
- Principle of Least Privilege
- Defense in Depth
- Immutable Infrastructure
- End-to-End Encryption
- Secure Origin Access
- Modular Terraform Code
- Resource Tagging
- Version Control
- Reusable Infrastructure

# Validation

The deployment was validated by confirming:

- Website accessible via custom domain.
- HTTPS certificate issued successfully.
- CloudFront distribution deployed.
- DNS resolution functioning correctly.
- CloudFront successfully serving cached content.
- Direct access to the S3 bucket blocked.
- Objects encrypted at rest.
- Terraform deployment completed successfully.

# Future Improvements

Potential enhancements include:

- GitHub Actions CI/CD pipeline
- AWS WAF integration
- AWS Shield Advanced
- CloudWatch dashboards
- CloudTrail auditing
- AWS Config compliance monitoring
- Multi-environment deployments
- Terraform remote backend with state locking
- Automated security scanning

# Lessons Learned

Through this project, I gained practical experience designing secure cloud infrastructure using AWS managed services and Infrastructure as Code.

Key learning outcomes include:

- Designing production-ready cloud architectures.
- Building reusable Terraform modules.
- Configuring secure static website hosting.
- Implementing CloudFront Origin Access Control.
- Managing DNS using Route 53.
- Configuring HTTPS using AWS Certificate Manager.
- Applying AWS security best practices.
- Understanding immutable infrastructure deployments.

# References

AWS Documentation

- Amazon S3
- Amazon CloudFront
- Amazon Route 53
- AWS Certificate Manager
- Terraform AWS Provider
