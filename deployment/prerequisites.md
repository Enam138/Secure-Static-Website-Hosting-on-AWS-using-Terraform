# Prerequisites

## Introduction

Before deploying the Akwannya Hub static website infrastructure, several prerequisites must be met to ensure a successful and repeatable deployment.

This project provisions AWS infrastructure using **Terraform** and deploys a secure static website architecture consisting of Amazon S3, Amazon CloudFront, Amazon Route 53, and AWS Certificate Manager (ACM).

This document outlines the required software, AWS resources, permissions, and configurations needed before deployment.

# Required Knowledge

Although Terraform automates the deployment process, a basic understanding of the following technologies is recommended:

- Amazon Web Services (AWS)
- Infrastructure as Code (IaC)
- Terraform
- DNS Concepts
- Static Website Hosting
- Git and GitHub
- Command Line Interface (CLI)

# AWS Account

An active AWS account is required.

The AWS account should have sufficient permissions to create and manage the following services:

- Amazon S3
- Amazon CloudFront
- Amazon Route 53
- AWS Certificate Manager
- AWS Identity and Access Management (IAM)

It is recommended to use an IAM user or IAM role with appropriate permissions rather than the AWS account root user.

# Required IAM Permissions

The deploying identity should have permissions similar to the following:

- Amazon S3
- Amazon CloudFront
- Amazon Route 53
- AWS Certificate Manager
- IAM (where applicable)
- CloudFront Origin Access Control

Following the Principle of Least Privilege, only the permissions required to provision and manage the infrastructure should be granted.

# Required Software

## Terraform

Terraform is used to provision all AWS resources.

Verify that Terraform is installed by running:

```bash
terraform version
```

A recent stable version of Terraform is recommended to ensure compatibility with the AWS Provider.

## AWS CLI

The AWS Command Line Interface (AWS CLI) is used to authenticate Terraform with your AWS account and perform optional management tasks.

Verify the installation:

```bash
aws --version
```

## Git

Git is required to clone and manage the project repository.

Verify installation:

```bash
git --version
```

# AWS CLI Configuration

Before deploying the infrastructure, configure the AWS CLI with credentials that have the required permissions.

Run:

```bash
aws configure
```

You will be prompted to enter:

- AWS Access Key ID
- AWS Secret Access Key
- Default AWS Region
- Output Format

After configuration, verify your identity:

```bash
aws sts get-caller-identity
```

This confirms that Terraform will authenticate using the correct AWS account.


# Domain Name

A registered domain name is required.

For this project, the custom domain is used to provide users with a friendly URL for accessing the Akwannya Hub website.

The domain must be managed through Amazon Route 53 or delegated to a Route 53 Hosted Zone to support DNS validation and routing.

# Route 53 Hosted Zone

A Hosted Zone must exist for the selected domain.

The Hosted Zone is responsible for:

- Managing DNS records
- Validating ACM certificates
- Routing traffic to Amazon CloudFront

Ensure that the domain's name servers are correctly configured to point to the Route 53 Hosted Zone.

# AWS Certificate Manager (ACM)

An SSL/TLS certificate is required to enable HTTPS.

The certificate should:

- Match the custom domain.
- Be validated using DNS.
- Be issued in the AWS Region required by Amazon CloudFront (US East (N. Virginia) for global distributions).

Once validated, the certificate can be attached to the CloudFront distribution.

# Terraform Project Structure

Before deployment, verify that the project directory follows the expected structure.

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
├── modules/
├── main.tf
├── variables.tf
├── outputs.tf
├── providers.tf
└── terraform.tfvars
```

Organizing Terraform files in a predictable structure improves maintainability and collaboration.

# Website Content

Prepare the static website assets before deployment.

Typical files include:

- index.html
- about.html
- styles.css
- JavaScript files
- Images
- Logos
- Icons

These assets will be uploaded to the private Amazon S3 bucket after the infrastructure has been provisioned.

# Internet Connectivity

Ensure that the deployment environment has reliable internet access.

Terraform communicates with AWS APIs throughout the provisioning process.

An unstable connection may interrupt deployments or delay resource creation.

# Estimated Deployment Time

The approximate deployment time is:

| Component | Estimated Time |
|-----------|---------------:|
| Amazon S3 | 1–2 minutes |
| ACM Certificate Request | 2–10 minutes (depending on DNS validation) |
| CloudFront Distribution | 10–20 minutes |
| Route 53 Configuration | 2–5 minutes |
| Terraform Provisioning | 15–30 minutes (overall) |

CloudFront deployment typically requires the longest amount of time because the distribution must propagate across AWS edge locations.

# Cost Considerations

This project is designed to use cost-effective AWS managed services.

However, deploying the infrastructure may incur charges depending on your AWS account usage.

Potential billable services include:

- Amazon S3 storage
- CloudFront requests and data transfer
- Route 53 Hosted Zone
- DNS queries

AWS Certificate Manager does not charge for public SSL/TLS certificates used with supported AWS services such as CloudFront.

Always review AWS pricing before deploying resources to a production environment.

# Pre-Deployment Checklist

Before running Terraform, verify the following:

- AWS account is active.
- AWS CLI is installed and configured.
- Terraform is installed.
- Git is installed.
- Domain name is registered.
- Route 53 Hosted Zone exists.
- ACM certificate has been requested.
- DNS validation records are configured.
- Website files are ready.
- Terraform configuration has been reviewed.
- IAM permissions follow the Principle of Least Privilege.

# Summary

Completing these prerequisites ensures that the deployment process is smooth, secure, and repeatable.

By verifying software installations, AWS account configuration, domain ownership, IAM permissions, and Terraform project structure beforehand, infrastructure provisioning can be automated with minimal manual intervention while adhering to AWS best practices.
