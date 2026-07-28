# Architecture Overview

## Introduction

This document provides a detailed overview of the cloud architecture designed and implemented for the **Akwannya Hub** website. The solution leverages fully managed AWS services to provide a secure, scalable, highly available, and cost-effective platform for hosting a static website.

The architecture follows the principles of the **AWS Well-Architected Framework**, emphasizing operational excellence, security, reliability, performance efficiency, cost optimization, and sustainability. By using Infrastructure as Code (IaC) with Terraform, the entire infrastructure can be deployed consistently, managed efficiently, and reproduced across environments.

Unlike traditional web hosting solutions that require virtual machines, operating system management, and web server maintenance, this architecture adopts a **serverless approach**, eliminating infrastructure management while benefiting from the scalability and resilience of AWS managed services.

# Architecture Objectives

The primary objectives of the architecture were to:

- Host the Akwannya Hub website using a fully managed cloud-native solution.
- Deliver content securely over HTTPS.
- Improve website performance through global edge caching.
- Eliminate the need for traditional web servers.
- Restrict direct access to the website's storage layer.
- Implement Infrastructure as Code using Terraform.
- Build reusable Terraform modules.
- Reduce operational overhead and infrastructure costs.
- Follow AWS security and architectural best practices.

# High-Level Architecture

The solution consists of five core AWS services that work together to securely deliver website content to users around the world.

<img width="1536" height="1024" alt="akwannya" src="https://github.com/user-attachments/assets/45a72770-5324-4086-b020-655d1126ef77" />


Terraform provisions and manages every component shown in the architecture.


# Architecture Components

## 1. Website Visitors

Website users access the Akwannya Hub website by entering the custom domain in a web browser.

Requests are made over HTTPS to ensure encrypted communication between users and AWS CloudFront.

No user communicates directly with Amazon S3.

## 2. Amazon Route 53

Amazon Route 53 provides authoritative Domain Name System (DNS) services for the Akwannya Hub domain.

### Responsibilities

- Domain name resolution
- Hosted Zone management
- Alias record configuration
- Routing traffic to CloudFront
- Highly available DNS service

When users enter the website URL, Route 53 resolves the domain and directs traffic to the CloudFront distribution instead of directly exposing the storage bucket.

### Benefits

- Global Anycast DNS
- Highly available infrastructure
- Low latency DNS resolution
- Native AWS integration

## 3. AWS Certificate Manager (ACM)

AWS Certificate Manager manages the SSL/TLS certificate used by CloudFront.

The certificate encrypts communication between website visitors and CloudFront.

### Responsibilities

- SSL certificate provisioning
- Certificate validation
- Automatic renewal
- HTTPS support

Using ACM eliminates the operational burden of manually issuing, renewing, and managing certificates.

### Benefits

- Automatic certificate renewal
- No additional cost
- Native CloudFront integration
- Strong encryption

## 4. Amazon CloudFront

Amazon CloudFront serves as the global Content Delivery Network (CDN) for the website.

Rather than retrieving content from Amazon S3 for every request, CloudFront caches website assets at AWS edge locations around the world.

### Responsibilities

- HTTPS termination
- Global caching
- Content delivery
- Request optimization
- Performance improvement
- Secure origin communication

CloudFront retrieves content from the S3 bucket only when the requested object is not already cached.

### Benefits

- Reduced latency
- Faster page loading
- Lower S3 request costs
- Automatic scaling
- Global edge network
- Improved user experience

## 5. Origin Access Control (OAC)

Origin Access Control (OAC) is one of the most important security components in the architecture.

Instead of allowing public access to the Amazon S3 bucket, OAC ensures that **only the CloudFront distribution** can retrieve website content.

This prevents users from bypassing CloudFront and accessing the bucket directly.

### Responsibilities

- Authenticate CloudFront requests
- Restrict S3 access
- Secure communication between CloudFront and Amazon S3

### Benefits

- Private origin
- Reduced attack surface
- Least privilege access
- Enhanced security

## 6. Amazon S3

Amazon S3 stores all static website assets.

Examples include:

- HTML files
- CSS files
- JavaScript
- Images
- Fonts
- Documents

Unlike traditional static website hosting, the bucket is configured as a **private origin**.

### Security Configuration

- Block Public Access enabled
- Bucket Versioning enabled
- Server-Side Encryption (SSE-S3)
- Bucket Policy allowing only CloudFront access
- Origin Access Control enabled

### Benefits

- Highly durable storage (99.999999999%)
- Automatic scalability
- Low cost
- High availability
- Secure object storage

# End-to-End Request Flow

The following sequence illustrates how a request is processed.

## Step 1

A user enters the Akwannya Hub website URL in a browser.

Example:

```
https://www.akwannyahub.com
```

## Step 2

Amazon Route 53 receives the DNS request.

The hosted zone resolves the domain name and returns the CloudFront distribution.

## Step 3

The browser establishes an HTTPS connection with CloudFront.

CloudFront presents the SSL/TLS certificate issued through AWS Certificate Manager.

## Step 4

CloudFront checks its edge cache.

If the requested object already exists:

- CloudFront immediately serves the cached content.

If not:

- CloudFront forwards the request to Amazon S3.

## Step 5

CloudFront authenticates itself using Origin Access Control.

Amazon S3 validates the request.

Since CloudFront is authorized by the bucket policy, access is granted.

## Step 6

Amazon S3 returns the requested object to CloudFront.

CloudFront caches the object for future requests.

## Step 7

CloudFront delivers the content securely to the user over HTTPS.

The website loads with low latency regardless of the user's geographic location.

# Security Architecture

Security was a primary design consideration throughout the implementation.

The architecture incorporates multiple layers of protection.

## Network Security

- HTTPS enforced
- TLS 1.2+
- CloudFront Viewer Certificate

## Storage Security

- Private S3 bucket
- Block Public Access
- Bucket Policy restrictions
- Server-side encryption
- Versioning enabled

## Access Control

- Origin Access Control
- Least privilege permissions
- IAM-based authentication
- Restricted bucket access

## Data Protection

Data is protected:

### In Transit

- HTTPS
- TLS encryption

### At Rest

- Amazon S3 Server-Side Encryption (SSE-S3)

# Scalability

The solution is designed to scale automatically.

### Amazon S3

Provides virtually unlimited storage capacity.

### CloudFront

Automatically scales to accommodate increasing traffic volumes.

### Route 53

Supports high query volumes using AWS's globally distributed DNS infrastructure.

No manual scaling is required.

# High Availability

The architecture provides high availability through AWS managed services.

Key characteristics include:

- Global CloudFront edge locations
- Amazon S3 regional durability
- Route 53 highly available DNS
- Fully managed services
- No single server dependency

# Performance Optimization

Several optimizations improve website performance.

## CloudFront Edge Caching

Frequently requested objects are cached closer to users.

## Reduced Latency

Users connect to the nearest CloudFront edge location rather than directly to the AWS Region hosting the S3 bucket.

## Efficient Content Delivery

Only cache misses require requests to Amazon S3.

## HTTPS Optimization

CloudFront efficiently terminates HTTPS connections at edge locations, reducing origin load.

# Cost Optimization

The solution minimizes operational expenses by relying on serverless managed services.

Key cost benefits include:

- No EC2 instances
- No operating system maintenance
- No web server licensing
- Pay-for-use pricing
- Automatic scaling
- Reduced bandwidth costs through CloudFront caching

# Infrastructure as Code

All infrastructure components are provisioned using Terraform.

Benefits include:

- Version-controlled infrastructure
- Repeatable deployments
- Automation
- Modular architecture
- Reduced configuration drift
- Faster provisioning
- Easier disaster recovery

# Architecture Summary

This architecture demonstrates a secure and production-ready implementation of static website hosting on AWS.

By combining Amazon S3, CloudFront, Route 53, AWS Certificate Manager, and Terraform, the solution provides:

- Secure HTTPS communication
- Global content delivery
- Private object storage
- Automatic scalability
- High availability
- Cost-efficient hosting
- Infrastructure as Code
- AWS security best practices
