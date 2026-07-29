# Encryption

## Introduction

Protecting data through encryption is a fundamental component of cloud security. Encryption helps ensure that information remains confidential and cannot be read by unauthorized individuals, even if intercepted during transmission or accessed from storage.

The Akwannya Hub static website architecture implements encryption for both **data in transit** and **data at rest** using AWS managed security services. By relying on AWS-native encryption capabilities, the solution provides strong protection while minimizing operational complexity.


# Encryption Objectives

The encryption strategy for this project was designed to:

- Protect website content during transmission.
- Protect stored website assets.
- Ensure secure communication between users and AWS services.
- Reduce the risk of unauthorized data disclosure.
- Align with AWS security best practices.
- Eliminate manual certificate and key management where possible.

# Encryption Overview

The project uses two forms of encryption.

| Encryption Type | AWS Service | Purpose |
|-----------------|-------------|---------|
| Data in Transit | AWS Certificate Manager (ACM) + Amazon CloudFront | Encrypt communication between users and the website |
| Data at Rest | Amazon S3 Server-Side Encryption (SSE-S3) | Encrypt website assets stored in Amazon S3 |

Together, these controls ensure that data remains protected throughout its lifecycle.

# Data in Transit

## Overview

Data in transit refers to information moving between a user's browser and AWS services.

Without encryption, network traffic could potentially be intercepted by malicious actors.

To mitigate this risk, all traffic between website visitors and Amazon CloudFront is encrypted using HTTPS.

## HTTPS

The Akwannya Hub website is configured to use HTTPS exclusively.

HTTP requests are automatically redirected to HTTPS by the CloudFront distribution.

Benefits include:

- Confidentiality
- Integrity
- Authentication
- User trust
- Browser security compliance

## Transport Layer Security (TLS)

HTTPS is implemented using Transport Layer Security (TLS).

TLS provides:

- Encryption of transmitted data
- Verification of server identity
- Protection against tampering
- Secure session establishment

The CloudFront distribution is configured to support modern TLS versions and secure cipher suites.

# AWS Certificate Manager (ACM)

## Purpose

AWS Certificate Manager manages the SSL/TLS certificate associated with the Akwannya Hub domain.

The certificate enables CloudFront to establish trusted HTTPS connections with website visitors.

## Certificate Lifecycle

The certificate lifecycle consists of:

1. Certificate request
2. DNS validation
3. Certificate issuance
4. Deployment to CloudFront
5. Automatic renewal

Because ACM manages certificate renewal automatically, there is no need for manual certificate replacement.

## Benefits

Using ACM provides several advantages:

- Automatic certificate renewal
- Native AWS integration
- Reduced operational overhead
- No additional cost for supported AWS services
- Simplified certificate management

# Data at Rest

## Overview

Data at rest refers to information stored within AWS services.

For this project, all website assets are stored in Amazon S3.

Although the content is static, protecting stored objects remains an important security requirement.


# Amazon S3 Server-Side Encryption (SSE-S3)

## Purpose

Server-Side Encryption with Amazon S3 managed keys (SSE-S3) encrypts objects before they are written to disk.

When an authorized request is made, Amazon S3 automatically decrypts the object before returning it.

This process is completely transparent to users and applications.


## How It Works

When a file is uploaded:

1. Amazon S3 receives the object.
2. The object is encrypted.
3. The encrypted object is stored.
4. Metadata is maintained.
5. Upon retrieval, Amazon S3 decrypts the object automatically for authorized requests.

No changes are required to the website code.

## Benefits

SSE-S3 provides:

- Encryption at rest
- Automatic key management
- Minimal administrative overhead
- Native AWS integration
- Strong data protection


# Encryption Key Management

One of the advantages of SSE-S3 is that AWS manages the encryption keys.

Responsibilities handled by AWS include:

- Key generation
- Secure key storage
- Key rotation (where applicable)
- Key protection
- Key lifecycle management

This eliminates the need for manual cryptographic key administration.


# Encryption in the Architecture

The encryption workflow follows the sequence below.

```
Website Visitor

        │

 HTTPS (TLS)

        │

        ▼

Amazon CloudFront

        │

 ACM Certificate

        │

        ▼

Origin Access Control

        │

        ▼

Amazon S3

(Server-Side Encryption)
```

Data remains encrypted throughout transmission and protected while stored.


# Security Benefits

The implemented encryption strategy provides several security advantages.

## Confidentiality

Website traffic cannot be read by unauthorized third parties while in transit.


## Integrity

TLS helps ensure that website content is not modified while being transmitted.


## Authentication

The SSL/TLS certificate confirms the identity of the website, helping users verify they are connecting to the legitimate Akwannya Hub domain.


## Data Protection

Objects stored in Amazon S3 remain encrypted, reducing the risk of data exposure.

## Operational Simplicity

AWS-managed encryption services eliminate much of the complexity associated with certificate and key management.


# Encryption Best Practices

The project follows several AWS encryption best practices:

- HTTPS enforced for all user traffic.
- HTTP redirected to HTTPS.
- SSL/TLS certificates managed by ACM.
- Server-side encryption enabled on Amazon S3.
- Encryption enabled by default for stored objects.
- Automatic certificate renewal.
- AWS-managed encryption keys.
- No hardcoded certificates or keys.

# Encryption Validation

The following validation checks were performed after deployment.

| Validation Item | Expected Result | Status |
|-----------------|-----------------|--------|
| HTTPS enabled | Secure connection established |  Passed |
| Valid ACM certificate | Browser trusts certificate |  Passed |
| HTTP redirects to HTTPS | Users automatically use secure protocol |  Passed |
| Amazon S3 encryption enabled | Objects encrypted at rest |  Passed |
| Website accessible over HTTPS | Secure website delivery |  Passed |


# Compliance Considerations

Although this project is intended as a cloud engineering portfolio demonstration, the encryption controls align with security practices commonly recommended in industry standards and frameworks, including:

- AWS Well-Architected Framework (Security Pillar)
- AWS Security Best Practices
- CIS AWS Foundations Benchmark (relevant controls)
- General principles of ISO/IEC 27001 for protecting information through encryption


# Summary

Encryption plays a critical role in protecting both users and website assets within the Akwannya Hub architecture.

By combining HTTPS, TLS, AWS Certificate Manager, and Amazon S3 Server-Side Encryption, the solution safeguards data during transmission and while stored in AWS.

The use of AWS-managed encryption services simplifies administration while maintaining a strong security posture and supporting the overall goal of delivering a secure, reliable, and production-ready static website.
