# Deployment Validation

## Introduction

After deploying the Akwannya Hub static website infrastructure, a series of validation tests were performed to verify that all AWS resources were functioning as expected.

The validation process focused on confirming that the infrastructure was deployed correctly, the website was accessible over HTTPS, security controls were operating as intended, and the architecture aligned with AWS best practices.

The tests documented below provide confidence that the solution is production-ready and operating according to the design specifications.


# Validation Objectives

The deployment was validated to confirm that:

- Infrastructure was successfully provisioned by Terraform.
- Website content was accessible through the custom domain.
- HTTPS encryption was functioning correctly.
- Amazon CloudFront was serving website content.
- Amazon S3 remained private.
- Origin Access Control (OAC) was functioning correctly.
- DNS records resolved successfully.
- Website assets loaded correctly.
- Infrastructure followed the intended architecture.


# Validation Results

| Validation Item | Expected Result | Status |
|-----------------|-----------------|--------|
| Terraform deployment completed | Infrastructure provisioned successfully |  Passed |
| Amazon S3 bucket created | Bucket available |  Passed |
| Versioning enabled | Object versioning active |  Passed |
| Server-side encryption enabled | Objects encrypted at rest |  Passed |
| Block Public Access enabled | Public access blocked |  Passed |
| CloudFront distribution deployed | Distribution available |  Passed |
| Origin Access Control configured | Secure origin access |  Passed |
| ACM certificate issued | HTTPS certificate available |  Passed |
| Route 53 DNS resolution | Domain resolves correctly |  Passed |
| Website accessible | Website loads successfully |  Passed |
| HTTPS enabled | Secure browser connection |  Passed |
| Direct S3 access blocked | Access denied |  Passed |


# Validation Test 1 – Terraform Deployment

## Objective

Verify that Terraform successfully provisioned all required AWS resources.

### Validation Steps

1. Execute:

```bash
terraform apply
```

2. Confirm that Terraform completed without errors.

3. Review the Terraform output.

### Expected Result

Terraform reports: Apply complete!


All planned resources are successfully created.

 Result: Passed

# Validation Test 2 – Amazon S3 Configuration

## Objective

Verify that the website bucket is configured according to the project requirements.

### Validation Steps

Review the bucket configuration within the AWS Management Console.

Confirm that:

- Bucket exists.
- Versioning is enabled.
- Server-side encryption is enabled.
- Block Public Access is enabled.
- Bucket policy is configured.

### Expected Result

The bucket remains private and only Amazon CloudFront is authorized to retrieve objects.

 Result: Passed

# Validation Test 3 – CloudFront Distribution

## Objective

Verify that Amazon CloudFront is correctly serving website content.

### Validation Steps

1. Open the CloudFront distribution.
2. Confirm the deployment status is **Deployed**.
3. Review the distribution settings.

### Expected Result

- Distribution status is **Deployed**.
- Distribution state is **Enabled**.
- Origin points to the Amazon S3 bucket.
- Origin Access Control is attached.

 Result: Passed

# Validation Test 4 – HTTPS Configuration

## Objective

Verify that HTTPS is configured correctly.

### Validation Steps

Open the website using the custom domain.

Example:

```
https://www.akwannyahub.com
```

Inspect the browser security information.

### Expected Result

- HTTPS connection established.
- Valid SSL/TLS certificate presented.
- No browser security warnings.

 Result: Passed

# Validation Test 5 – Route 53 DNS Resolution

## Objective

Verify that DNS resolves correctly.

### Validation Steps

Access the website using the custom domain.

Alternatively, use:

```bash
nslookup www.akwannyahub.com
```

or

```bash
dig www.akwannyahub.com
```

### Expected Result

The domain resolves to the CloudFront distribution.

 Result: Passed

# Validation Test 6 – Origin Access Control

## Objective

Verify that only CloudFront can access the Amazon S3 bucket.

### Validation Steps

Attempt to access a website object directly through the Amazon S3 object URL.

### Expected Result

Amazon S3 returns an **Access Denied** response.

Website content remains accessible through CloudFront.

 Result: Passed

# Validation Test 7 – Website Content

## Objective

Verify that website assets load successfully.

### Validation Steps

Navigate through the website.

Review:

- Home page
- Images
- CSS styling
- JavaScript functionality
- Navigation
- Static assets

### Expected Result

All website components load successfully without errors.

 Result: Passed

# Validation Test 8 – CloudFront Caching

## Objective

Verify that Amazon CloudFront caches static content.

### Validation Steps

Access the website multiple times from the browser.

Inspect the HTTP response headers using the browser's Developer Tools or a command such as:

```bash
curl -I https://www.akwannyahub.com
```

### Expected Result

Responses indicate that CloudFront is serving the content and caching behavior is active.

 Result: Passed

# Security Validation

The following security controls were verified during deployment.

| Security Control | Status |
|------------------|--------|
| HTTPS enforced | Done |
| TLS encryption | Done |
| Private Amazon S3 bucket | Done |
| Block Public Access | Done |
| Origin Access Control | Done |
| Bucket Policy restrictions | Done |
| Server-side encryption | Done |
| Least-privilege access | Done |


# Performance Validation

The deployment was reviewed against the intended performance objectives.

| Performance Requirement | Result |
|-------------------------|--------|
| Global content delivery | Done |
| CloudFront edge caching | Done |
| Low-latency content delivery | Done |
| Automatic scaling | Done |
| Static asset optimization | Done |

# Operational Validation

Operational readiness was confirmed through the following checks.

| Operational Check | Status |
|-------------------|--------|
| Infrastructure managed by Terraform | Done |
| Infrastructure reproducible | Done |
| Modular Terraform design | Done |
| Infrastructure documented | Done |
| Rollback procedure available | Done |


# Validation Summary

The deployment validation confirms that the Akwannya Hub static website infrastructure was successfully implemented according to the project requirements.

All core AWS services—including Amazon S3, Amazon CloudFront, Amazon Route 53, and AWS Certificate Manager—were verified to be functioning correctly. Security controls such as Origin Access Control, Block Public Access, HTTPS, and server-side encryption were also validated.

The infrastructure provides a secure, scalable, highly available, and production-ready platform for hosting the Akwannya Hub website while adhering to AWS Well-Architected Framework principles and Infrastructure as Code best practices.
