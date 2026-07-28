# Deployment Guide

## Introduction

This guide describes the deployment process for the **Akwannya Hub** static website infrastructure using Terraform.

The deployment provisions a secure, scalable, and production-ready static website architecture on Amazon Web Services (AWS). All infrastructure components are created through Infrastructure as Code (IaC), ensuring that deployments are repeatable, consistent, and version-controlled.

The deployment includes:

- Amazon S3
- Amazon CloudFront
- Amazon Route 53
- AWS Certificate Manager (ACM)
- Origin Access Control (OAC)
- IAM Policies
- DNS Configuration


# Deployment Workflow

The deployment follows the sequence below.

```
Clone Repository

        │

        ▼

Configure AWS CLI

        │

        ▼

Initialize Terraform

        │

        ▼

Validate Configuration

        │

        ▼

Generate Execution Plan

        │

        ▼

Deploy Infrastructure

        │

        ▼

Upload Website Files

        │

        ▼

Validate Deployment

        │

        ▼

Website Available via HTTPS
```


# Step 1 – Clone the Repository

Clone the project repository to your local machine.

```bash
git clone https://github.com/<your-github-username>/aws-static-website-terraform.git

cd aws-static-website-terraform
```

Verify that the repository structure matches the project documentation.


# Step 2 – Configure AWS Credentials

Terraform authenticates with AWS using the AWS CLI configuration.

Configure your credentials:

```bash
aws configure
```

Provide:

- AWS Access Key ID
- AWS Secret Access Key
- Default Region
- Output Format

Verify the authenticated account:

```bash
aws sts get-caller-identity
```

The returned account information should match the AWS account where the infrastructure will be deployed.


# Step 3 – Review Terraform Variables

Review the variables that define the deployment.

Typical variables include:

- AWS Region
- Domain Name
- Bucket Name
- CloudFront Price Class
- Resource Tags

If a `terraform.tfvars` file is used, ensure the values reflect the target environment before deployment.


# Step 4 – Initialize Terraform

Initialize the working directory.

```bash
terraform init
```

Terraform performs the following tasks:

- Downloads required providers
- Initializes backend configuration
- Downloads referenced modules
- Creates the local `.terraform` directory

A successful initialization indicates that Terraform is ready to manage the infrastructure.


# Step 5 – Validate the Configuration

Validate the Terraform configuration.

```bash
terraform validate
```

Terraform checks:

- Syntax
- Resource definitions
- Module references
- Variable usage

Any validation errors should be resolved before continuing.


# Step 6 – Review the Execution Plan

Generate an execution plan.

```bash
terraform plan
```

The execution plan displays:

- Resources to be created
- Resources to be modified
- Resources to be destroyed
- Estimated infrastructure changes

Review the plan carefully to confirm that only the intended resources will be provisioned.


# Step 7 – Deploy the Infrastructure

Deploy the AWS resources.

```bash
terraform apply
```

Terraform will display a summary of the planned changes.

Type:

```text
yes
```

to begin the deployment.

Terraform then provisions the infrastructure in the correct dependency order.


# Resources Created

The deployment provisions resources similar to the following:

## Amazon S3

- Website bucket
- Versioning
- Server-side encryption
- Bucket policy
- Block Public Access


## AWS Certificate Manager

- Public SSL/TLS certificate
- DNS validation records


## Amazon CloudFront

- Distribution
- Viewer Certificate
- Origin Access Control
- Cache behavior

## Amazon Route 53

- Alias record
- DNS validation records


# Step 8 – Wait for Resource Provisioning

Some AWS services require additional time after Terraform completes.

Typical propagation times include:

| Service | Estimated Time |
|----------|---------------:|
| Amazon S3 | 1–2 minutes |
| Route 53 | 2–5 minutes |
| ACM Validation | 2–10 minutes |
| CloudFront Distribution | 10–20 minutes |

CloudFront deployment is generally the longest step because the distribution must propagate to edge locations worldwide.

# Step 9 – Upload Website Content

After the infrastructure is available, upload the website files to the Amazon S3 bucket.

Typical website assets include:

- HTML files
- CSS files
- JavaScript files
- Images
- Fonts
- Logos

Once uploaded, CloudFront automatically serves the content through the configured distribution.

# Step 10 – Validate HTTPS

Open the website using the custom domain.

Example:

```
https://www.akwannyahub.com
```

Verify that:

- HTTPS is enabled.
- The browser reports a valid certificate.
- No certificate warnings are displayed.

The SSL/TLS certificate should be issued by AWS Certificate Manager and presented by CloudFront.

# Step 11 – Verify DNS Resolution

Confirm that the custom domain resolves to the CloudFront distribution.

Validation includes:

- Domain resolves successfully.
- Route 53 Alias Record is functioning.
- CloudFront distribution is reachable.

# Step 12 – Verify CloudFront Caching

Access the website multiple times.

Observe that:

- Website assets load quickly.
- Images are cached.
- CSS and JavaScript files are served efficiently.

CloudFront should serve cached content from the nearest edge location whenever possible.

# Step 13 – Verify S3 Security

Attempt to access an object directly through the Amazon S3 URL.

Expected behavior:

- Access is denied.
- The bucket is not publicly accessible.
- Only CloudFront can retrieve objects.

This confirms that Origin Access Control and the bucket policy are functioning correctly.

# Step 14 – Review Terraform Outputs

Display deployment outputs.

```bash
terraform output
```

Typical outputs include:

- Website URL
- CloudFront Distribution ID
- Distribution Domain Name
- S3 Bucket Name
- Certificate ARN

These outputs provide useful deployment information for validation and future management.

# Post-Deployment Checklist

After deployment, confirm the following:

- Terraform completed successfully.
- Amazon S3 bucket created.
- Versioning enabled.
- Server-side encryption enabled.
- Block Public Access enabled.
- CloudFront distribution deployed.
- Origin Access Control configured.
- ACM certificate issued.
- Route 53 Alias Record configured.
- Website accessible over HTTPS.
- Website assets served through CloudFront.
- Direct S3 access blocked.

# Rollback Procedure

If the infrastructure is no longer required, Terraform can remove all managed resources.

Preview the destruction plan:

```bash
terraform plan -destroy
```

Destroy the infrastructure:

```bash
terraform destroy
```

Terraform removes the resources it manages in the correct dependency order.

> **Note:** Ensure that any important website content or configuration is backed up before destroying the infrastructure.


# Deployment Summary

The deployment process provisions a secure, production-ready static website architecture using Terraform and AWS managed services.

By following this guide, the Akwannya Hub website can be deployed consistently across environments while maintaining strong security, high availability, and operational efficiency.

The combination of Terraform, Amazon S3, CloudFront, Route 53, AWS Certificate Manager, and Origin Access Control provides a modern cloud-native solution that follows AWS best practices and Infrastructure as Code principles.
