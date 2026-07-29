# Threat Model

## Introduction

A threat model is a structured approach to identifying potential security threats, evaluating their impact, and implementing appropriate mitigations.

For the Akwannya Hub static website architecture, threat modeling was used during the design phase to identify risks associated with hosting a public website on AWS and to ensure that appropriate security controls were implemented.

The objective was to minimize the attack surface while maintaining a secure, highly available, and scalable architecture.

# Threat Modeling Objectives

The threat model was developed to:

- Identify critical assets.
- Identify potential attackers.
- Analyze possible attack vectors.
- Evaluate security risks.
- Document implemented mitigations.
- Identify any remaining residual risks.

# System Overview

The architecture consists of the following AWS services:

- Amazon Route 53
- Amazon CloudFront
- AWS Certificate Manager (ACM)
- Origin Access Control (OAC)
- Amazon S3
- Terraform

Website traffic flows through CloudFront before reaching the private S3 bucket, ensuring that the storage layer is never exposed directly to the internet.

# Protected Assets

The following assets were considered during the threat modeling process.

| Asset | Importance |
|--------|------------|
| Website content | High |
| Amazon S3 bucket | High |
| CloudFront distribution | High |
| Route 53 DNS records | Medium |
| ACM certificates | High |
| Terraform configuration | High |
| AWS credentials | Critical |

# Potential Threat Actors

Potential threat actors include:

- Anonymous internet users
- Malicious attackers
- Automated bots
- Credential thieves
- Insider threats
- Misconfigured administrators

Each actor presents different risks depending on their level of access and intent.

# Threat Categories

The following categories were considered during the security review.

- Unauthorized access
- Data exposure
- Credential compromise
- Misconfiguration
- Denial of Service (DoS)
- Traffic interception
- Infrastructure tampering

# Threat Analysis

## Threat 1 – Public Amazon S3 Bucket

### Description

If the Amazon S3 bucket were configured for public access, website assets could be accessed directly without CloudFront.

### Potential Impact

- Information disclosure
- Unauthorized downloads
- Increased attack surface

### Mitigation

- Block Public Access enabled
- Private S3 bucket
- Bucket Policy
- Origin Access Control

### Residual Risk

Low

## Threat 2 – Direct Origin Access

### Description

Attackers may attempt to bypass CloudFront by accessing Amazon S3 directly.

### Potential Impact

- Circumvention of security controls
- Exposure of website objects

### Mitigation

- Origin Access Control (OAC)
- Bucket policy restricting access to CloudFront
- Private S3 bucket

### Residual Risk

Low

## Threat 3 – Traffic Interception

### Description

Without encryption, website traffic could be intercepted during transmission.

### Potential Impact

- Credential theft (where applicable)
- Information disclosure
- Session manipulation
- Man-in-the-Middle (MITM) attacks

### Mitigation

- HTTPS enforced
- TLS
- AWS Certificate Manager
- CloudFront Viewer Certificate

### Residual Risk

Low

## Threat 4 – Accidental Data Loss

### Description

Website files could be unintentionally modified or deleted.

### Potential Impact

- Website outage
- Loss of content
- Recovery delays

### Mitigation

- Amazon S3 Versioning
- Terraform Infrastructure as Code
- Source code stored in version control

### Residual Risk

Low

## Threat 5 – AWS Credential Compromise

### Description

Compromised AWS credentials could allow unauthorized administrative access.

### Potential Impact

- Resource deletion
- Infrastructure modification
- Financial impact
- Complete environment compromise

### Mitigation

- IAM
- Principle of Least Privilege
- Multi-Factor Authentication (MFA)
- Secure credential management
- No secrets stored in Git repositories

### Residual Risk

Medium

Credential security ultimately depends on operational practices and user behavior.

## Threat 6 – Infrastructure Misconfiguration

### Description

Manual changes could introduce security weaknesses.

### Potential Impact

- Public resource exposure
- Service disruption
- Configuration drift

### Mitigation

- Terraform
- Infrastructure as Code
- Version-controlled configuration
- Peer review before deployment

### Residual Risk

Low

## Threat 7 – DNS Misconfiguration

### Description

Incorrect DNS records could prevent users from accessing the website.

### Potential Impact

- Service outage
- Loss of availability
- User confusion

### Mitigation

- Amazon Route 53 Hosted Zone
- Terraform-managed DNS records
- Validation testing

### Residual Risk

Low

## Threat 8 – Distributed Denial of Service (DDoS)

### Description

Attackers may attempt to overwhelm the website with excessive traffic.

### Potential Impact

- Reduced performance
- Service disruption
- Increased operational costs

### Mitigation

- Amazon CloudFront
- AWS Shield Standard (enabled automatically for CloudFront)
- Globally distributed edge locations

### Residual Risk

Medium

Large-scale DDoS attacks remain a potential risk, although AWS provides substantial built-in protections.

# Threat Summary

| Threat | Likelihood | Impact | Risk Level | Mitigation |
|----------|------------|---------|------------|------------|
| Public S3 Bucket | Low | High | Low | Block Public Access, OAC |
| Direct Origin Access | Low | High | Low | OAC, Bucket Policy |
| Traffic Interception | Low | High | Low | HTTPS, TLS, ACM |
| Data Loss | Medium | Medium | Low | Versioning, Terraform |
| Credential Compromise | Medium | High | Medium | IAM, MFA, Least Privilege |
| Infrastructure Misconfiguration | Medium | Medium | Low | Terraform |
| DNS Misconfiguration | Low | Medium | Low | Route 53 |
| DDoS Attack | Medium | High | Medium | CloudFront, AWS Shield Standard |


# Defense-in-Depth Strategy

Rather than relying on a single security control, the architecture implements multiple independent layers of protection.

| Security Layer | Control |
|----------------|---------|
| DNS | Amazon Route 53 |
| Transport | HTTPS / TLS |
| Certificate Management | AWS Certificate Manager |
| Content Delivery | Amazon CloudFront |
| Origin Protection | Origin Access Control |
| Storage | Private Amazon S3 |
| Access Control | Bucket Policy |
| Encryption | Server-Side Encryption |
| Identity | IAM |
| Infrastructure | Terraform |

If one control were to fail, additional layers continue to reduce overall risk.

# Residual Risks

Despite the implemented controls, some risks cannot be completely eliminated.

Examples include:

- Compromised administrator credentials
- Large-scale DDoS attacks
- Third-party DNS issues
- Human error during future changes
- AWS regional service disruptions

These risks can be further reduced through continuous monitoring, periodic security reviews, least-privilege access management, and adherence to AWS operational best practices.

# Alignment with Security Frameworks

The threat model aligns with widely recognized security principles, including:

- AWS Well-Architected Framework (Security Pillar)
- Principle of Least Privilege
- Defense in Depth
- Secure by Design
- Infrastructure as Code (IaC)

# Summary

The threat modeling process helped identify the primary security risks associated with hosting the Akwannya Hub website on AWS and informed the selection of appropriate security controls.

By combining private storage, Origin Access Control, HTTPS, encryption, IAM best practices, and Infrastructure as Code, the architecture reduces the likelihood and impact of common threats while providing a secure and resilient hosting environment.

Although no system can eliminate all risks, the implemented controls establish a strong security baseline that supports secure, reliable, and maintainable operations.
