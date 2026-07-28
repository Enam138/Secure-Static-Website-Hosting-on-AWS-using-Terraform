# Security Controls

## Introduction

Security was a fundamental design consideration throughout the implementation of the Akwannya Hub static website architecture. Rather than simply hosting static website content, the solution was designed to protect the confidentiality, integrity, and availability of website assets while following AWS security best practices and the AWS Well-Architected Framework Security Pillar.

The architecture applies multiple layers of security to reduce the attack surface, enforce least-privilege access, encrypt data, and prevent unauthorized access to the origin storage.

Instead of relying on a single security mechanism, the solution adopts a **defense-in-depth** strategy where multiple security controls work together to protect the application.

# Security Objectives

The security design was guided by the following objectives:

- Protect website content from unauthorized access.
- Encrypt data during transmission.
- Encrypt stored data at rest.
- Prevent direct access to the Amazon S3 bucket.
- Ensure that only trusted AWS services can access the origin.
- Minimize exposed attack surfaces.
- Implement Infrastructure as Code for consistent security configurations.
- Follow AWS security best practices.

# Security Architecture

The security model consists of several integrated AWS services.

```
Website Visitor
        │
 HTTPS (TLS)
        │
        ▼
Amazon CloudFront
        │
 Origin Access Control
        │
        ▼
Private Amazon S3 Bucket
        │
Server-Side Encryption
        │
Block Public Access
```

Each component contributes to protecting the overall solution.


# Security Control 1 – HTTPS Encryption

## Objective

Protect data while it is transmitted between website visitors and AWS.

## Implementation

HTTPS is enforced using:

- Amazon CloudFront
- AWS Certificate Manager (ACM)
- TLS 1.2+

CloudFront terminates HTTPS connections using an ACM-issued SSL/TLS certificate.

## Threats Mitigated

- Eavesdropping
- Packet sniffing
- Session hijacking
- Data interception
- Man-in-the-Middle (MITM) attacks

## Benefits

- Encrypted communication
- Trusted browser connection
- Improved user confidence
- Industry-standard transport security

# Security Control 2 – AWS Certificate Manager (ACM)

## Objective

Provide secure SSL/TLS certificate management.

## Implementation

The project uses AWS Certificate Manager to:

- Request public certificates
- Validate domain ownership through DNS
- Automatically renew certificates
- Integrate directly with CloudFront

## Benefits

- Automatic certificate renewal
- No manual certificate management
- Native AWS integration
- Reduced operational overhead

# Security Control 3 – Private Amazon S3 Bucket

## Objective

Prevent public access to website content.

## Implementation

The website bucket is configured as a **private origin**.

Users never access Amazon S3 directly.

Instead, all requests are routed through Amazon CloudFront.

## Threats Mitigated

- Unauthorized object downloads
- Direct bucket enumeration
- Public object exposure
- Misconfigured public access

## Benefits

- Reduced attack surface
- Improved access control
- Secure origin architecture

# Security Control 4 – Block Public Access

## Objective

Prevent accidental or intentional public exposure of the S3 bucket.

## Implementation

Amazon S3 Block Public Access is enabled at the bucket level.

This prevents:

- Public bucket policies
- Public Access Control Lists (ACLs)
- Public object ACLs
- Cross-account public exposure

## Benefits

- Protection against configuration mistakes
- Improved compliance with AWS security recommendations
- Reduced risk of data leakage

# Security Control 5 – Origin Access Control (OAC)

## Objective

Restrict access to Amazon S3 so that only the CloudFront distribution can retrieve website content.

## Implementation

Origin Access Control authenticates requests sent from CloudFront to Amazon S3.

The bucket policy explicitly permits access only from the associated CloudFront distribution.

All other requests are denied.

## Threats Mitigated

- Direct access to S3 objects
- Bypass of CloudFront
- Unauthorized downloads
- Origin exposure

## Benefits

- Private storage
- Strong origin authentication
- AWS-recommended replacement for Origin Access Identity (OAI)
- Improved security posture

# Security Control 6 – Bucket Policy

## Objective

Define exactly who can access website objects.

## Implementation

The bucket policy grants read access exclusively to the CloudFront distribution using Origin Access Control.

No anonymous users are permitted.

No public read permissions are assigned.

## Benefits

- Fine-grained access control
- Least-privilege permissions
- Reduced exposure

# Security Control 7 – Server-Side Encryption (SSE-S3)

## Objective

Protect website assets while stored in Amazon S3.

## Implementation

Server-Side Encryption with Amazon S3 managed keys (SSE-S3) is enabled for the bucket.

Objects are automatically encrypted before being written to storage and decrypted transparently when accessed by authorized services.

## Threats Mitigated

- Unauthorized access to stored data
- Data exposure from compromised storage media

## Benefits

- Encryption at rest
- Automatic key management
- Minimal operational overhead

# Security Control 8 – Amazon S3 Versioning

## Objective

Protect against accidental deletion or unintended modification of website assets.

## Implementation

Bucket versioning is enabled.

Every time an object is modified, Amazon S3 preserves the previous version.

## Benefits

- Recovery from accidental deletion
- Recovery from unintended updates
- Improved resilience
- Simplified rollback

# Security Control 9 – Infrastructure as Code

## Objective

Reduce security risks associated with manual infrastructure changes.

## Implementation

Terraform provisions all AWS resources.

Security configurations such as:

- Bucket policies
- Encryption
- Origin Access Control
- Versioning
- Block Public Access

are defined declaratively within Terraform configuration files.

## Benefits

- Repeatable deployments
- Version-controlled security
- Reduced configuration drift
- Easier auditing

# Security Control 10 – Least Privilege

## Objective

Ensure that AWS resources receive only the permissions required to perform their intended functions.

## Implementation

Permissions are limited so that:

- CloudFront can read website objects.
- Amazon S3 accepts requests only from CloudFront.
- Administrative access is managed through IAM.

## Benefits

- Reduced privilege escalation risk
- Smaller attack surface
- Improved compliance

# Defense-in-Depth

Rather than depending on a single control, the architecture combines multiple layers of protection.

| Security Layer | Control |
|----------------|---------|
| DNS | Amazon Route 53 |
| Transport Security | HTTPS / TLS |
| Certificate Management | AWS Certificate Manager |
| CDN | Amazon CloudFront |
| Origin Authentication | Origin Access Control |
| Storage Security | Private Amazon S3 Bucket |
| Access Control | Bucket Policy |
| Data Protection | Server-Side Encryption |
| Recovery | Versioning |
| Infrastructure Security | Terraform |

Each layer complements the others, creating a resilient security posture.

# Security Best Practices Implemented

The project incorporates several AWS security best practices, including:

- Principle of Least Privilege
- Defense in Depth
- Encryption in Transit
- Encryption at Rest
- Private Origin Architecture
- Infrastructure as Code
- Secure DNS Routing
- Managed SSL/TLS Certificates
- Automatic Certificate Renewal
- Secure Content Delivery through CloudFront

# Security Validation

The following controls were verified after deployment.

| Security Control | Status |
|------------------|--------|
| HTTPS Enabled |  Passed  |
| TLS Encryption |  Passed  |
| ACM Certificate Valid |  Passed  |
| Private S3 Bucket |  Passed  |
| Block Public Access Enabled |  Passed  |
| Origin Access Control Configured |  Passed  |
| Bucket Policy Applied |  Passed  |
| Server-Side Encryption Enabled |  Passed  |
| Versioning Enabled |  Passed  |
| Least-Privilege Access | Passed |

# Summary

The Akwannya Hub static website architecture applies multiple complementary security controls to protect website content and reduce operational risk.

By combining Amazon CloudFront, AWS Certificate Manager, Origin Access Control, Amazon S3 security features, and Infrastructure as Code with Terraform, the solution provides a secure, scalable, and production-ready hosting platform that aligns with AWS security best practices and modern cloud architecture principles.
