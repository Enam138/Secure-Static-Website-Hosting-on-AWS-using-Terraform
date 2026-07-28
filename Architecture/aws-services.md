# AWS Services Overview

## Introduction

The Akwannya Hub website is built using a collection of fully managed AWS services that work together to provide a secure, highly available, scalable, and cost-effective static website hosting solution.

Each AWS service was selected to perform a specific role within the overall architecture. Instead of relying on traditional web servers, the solution adopts a serverless architecture where AWS managed services handle infrastructure provisioning, scaling, availability, and security.

This document explains the purpose, configuration, security considerations, and integration of each AWS service used within the solution.

# Amazon S3

## Purpose

Amazon Simple Storage Service (Amazon S3) serves as the origin storage layer for the Akwannya Hub website.

All static website assets are stored inside a private S3 bucket.

These assets include:

- HTML pages
- CSS stylesheets
- JavaScript files
- Images
- Logos
- Icons
- Documents
- Downloadable resources

Unlike traditional static website hosting where S3 objects are publicly accessible, this project implements a **private bucket architecture**.

Only Amazon CloudFront can retrieve content from the bucket.

## Configuration

The S3 bucket was configured with the following security and availability settings:

- Private bucket
- Block Public Access enabled
- Versioning enabled
- Server-Side Encryption (SSE-S3)
- Bucket Policy
- Origin Access Control integration

Static Website Hosting was intentionally disabled because website requests are handled through Amazon CloudFront instead of directly through Amazon S3.

## Why Amazon S3?

Amazon S3 provides:

- Extremely high durability (99.999999999%)
- Automatic scalability
- Low operational cost
- Native integration with CloudFront
- Built-in encryption
- Object versioning
- Fine-grained access control

## Security Considerations

The storage layer follows AWS security best practices.

Implemented controls include:

- Block Public Access
- Private Bucket
- Bucket Policy
- Server-Side Encryption
- Versioning
- Access restricted to CloudFront using Origin Access Control

This significantly reduces the attack surface by preventing users from directly accessing website content stored within the bucket.

# Amazon CloudFront

## Purpose

Amazon CloudFront is the Content Delivery Network (CDN) responsible for delivering website content to users worldwide.

Rather than allowing users to connect directly to Amazon S3, CloudFront acts as the public entry point for the website.

It caches website assets at AWS Edge Locations around the world, improving performance and reducing latency.

## Configuration

CloudFront was configured with:

- Custom Domain Name
- ACM Viewer Certificate
- HTTPS Only
- Origin Access Control
- Default Cache Behavior
- Managed Cache Policy
- Compression
- HTTP to HTTPS Redirection

## Viewer Certificate

CloudFront uses an SSL/TLS certificate issued through AWS Certificate Manager.

This enables:

- HTTPS
- TLS 1.2+
- Secure browser communication

## Caching

CloudFront caches static website assets including:

- HTML
- CSS
- JavaScript
- Images
- Fonts

Frequently requested objects are served directly from edge locations without contacting the origin.

Benefits include:

- Lower latency
- Reduced origin traffic
- Faster page loading
- Lower Amazon S3 request costs

## Origin Access Control (OAC)

One of the most important security features implemented is Origin Access Control.

OAC authenticates CloudFront when retrieving objects from Amazon S3.

This allows the bucket to remain private while still serving website content.

Unlike legacy Origin Access Identity (OAI), OAC provides improved security and is the AWS-recommended approach for new CloudFront distributions.

# Amazon Route 53

## Purpose

Amazon Route 53 provides Domain Name System (DNS) services.

It maps the Akwannya Hub domain name to the CloudFront distribution.

Without Route 53, users would need to access the website using the CloudFront distribution URL instead of a custom domain.

## Configuration

Configured resources include:

- Hosted Zone
- Alias Record
- Optional DNSSEC support

The Alias Record points directly to the CloudFront distribution.

## Benefits

- Highly available DNS
- Global Anycast network
- Low latency lookups
- Native AWS integration
- Automatic failover capabilities

# AWS Certificate Manager (ACM)

## Purpose

AWS Certificate Manager provides the SSL/TLS certificate used by CloudFront.

The certificate secures communication between website visitors and CloudFront.

## Configuration

The certificate was:

- Requested for the Akwannya Hub domain
- DNS validated
- Attached to CloudFront as the Viewer Certificate
- Automatically renewed by AWS

## Benefits

- Free public certificates
- Automatic renewal
- Simplified certificate management
- Native CloudFront integration

# Terraform

## Purpose

Terraform was used as the Infrastructure as Code (IaC) tool for provisioning and managing the AWS environment.

Instead of manually creating AWS resources through the AWS Management Console, every infrastructure component is defined declaratively in Terraform configuration files.

This approach enables automation, consistency, version control, and repeatable deployments.

## Modular Design

The infrastructure is organized into reusable Terraform modules.

### Route53 Module

Responsible for:

- Hosted Zone
- Alias Records
- DNS configuration

Outputs include:

- Zone ID
- Name Servers

### ACM Module

Responsible for:

- Certificate creation
- DNS validation
- Certificate outputs

### CloudFront Module

Responsible for:

- Distribution creation
- Cache behavior
- Viewer certificate
- Origin Access Control

Outputs include:

- Distribution ID
- Distribution Domain Name

### S3 Website Module

Responsible for:

- Private bucket creation
- Versioning
- Encryption
- Bucket Policy
- OAC permissions

Outputs include:

- Bucket ARN
- Bucket Domain Name

# Terraform Data Sources

To improve portability and eliminate hardcoded values, the project makes use of Terraform data sources.

Examples include:

- aws_route53_zone
- aws_acm_certificate
- aws_region
- aws_caller_identity
- aws_cloudfront_cache_policy

Using dynamic lookups ensures the configuration remains reusable across AWS accounts and environments.

# Infrastructure Benefits

Using Terraform provides several operational advantages.

## Version Control

Infrastructure changes are tracked alongside application code.

## Repeatable Deployments

The same infrastructure can be deployed consistently across multiple environments.

## Automation

Infrastructure provisioning becomes automated, reducing manual configuration errors.

## Scalability

Additional resources or environments can be deployed quickly by reusing existing modules.

## Reduced Configuration Drift

Infrastructure remains consistent because changes are managed through Terraform rather than manual console updates.

# AWS Architecture Integration

The services interact as follows:

1. Users access the Akwannya Hub website through a custom domain.

2. Amazon Route 53 resolves the domain to the CloudFront distribution.

3. CloudFront establishes an encrypted HTTPS connection using the ACM certificate.

4. CloudFront checks whether the requested object exists in its cache.

5. If necessary, CloudFront securely retrieves the object from Amazon S3 using Origin Access Control.

6. Amazon S3 validates CloudFront's identity using the bucket policy.

7. The requested object is returned to CloudFront.

8. CloudFront caches the object and securely delivers it to the user.

# Summary

Each AWS service within the Akwannya Hub architecture has a clearly defined responsibility.

| Service | Responsibility |
|----------|----------------|
| Amazon S3 | Secure storage for static website assets |
| Amazon CloudFront | Global content delivery and caching |
| Amazon Route 53 | DNS management and domain resolution |
| AWS Certificate Manager | SSL/TLS certificate provisioning |
| Terraform | Infrastructure provisioning and lifecycle management |

Together, these services create a secure, resilient, and production-ready architecture that delivers the Akwannya Hub website with high availability, low latency, strong security, and minimal operational overhead.
