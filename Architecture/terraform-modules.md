# Terraform Modules

## Introduction

Infrastructure as Code (IaC) is a core principle of modern cloud engineering, enabling infrastructure to be defined, deployed, and managed through code rather than manual configuration.

For the Akwannya Hub static website, Terraform was used to provision and manage all AWS resources. Instead of creating resources individually through the AWS Management Console, the entire environment is described declaratively in Terraform configuration files.

This modular approach improves consistency, maintainability, scalability, and repeatability while reducing the risk of configuration drift.

# Why Terraform?

Terraform was selected because it provides a cloud-agnostic Infrastructure as Code framework that integrates seamlessly with AWS.

Using Terraform offers several advantages:

- Infrastructure can be version controlled using Git.
- Deployments are repeatable across environments.
- Resources are created in the correct dependency order.
- Infrastructure changes are easy to review before deployment.
- Configuration drift is minimized.
- Existing modules can be reused across future projects.

For this project, Terraform became the single source of truth for the AWS infrastructure.

# Infrastructure Architecture

The Terraform configuration provisions the following AWS resources:

```
Terraform

│

├── Amazon S3 Bucket

├── Bucket Versioning

├── Server-Side Encryption

├── Block Public Access

├── Bucket Policy

├── AWS Certificate Manager

├── DNS Validation Records

├── Amazon CloudFront Distribution

├── Origin Access Control (OAC)

└── Amazon Route 53 Alias Record
```

Terraform automatically determines the order in which these resources are created based on their dependencies.

# Modular Design

Rather than placing every resource inside a single Terraform file, the infrastructure is organized into reusable modules.

Each module is responsible for provisioning one logical component of the architecture.

This improves readability, maintenance, and reusability.

# S3 Module

## Purpose

The S3 module provisions the storage layer used to host the Akwannya Hub website.

### Resources Managed

- Amazon S3 Bucket
- Versioning
- Server-Side Encryption
- Bucket Policy
- Block Public Access

### Responsibilities

- Create the website bucket
- Configure security settings
- Enable versioning
- Restrict public access
- Allow CloudFront access using Origin Access Control

### Outputs

The module exports values including:

- Bucket Name
- Bucket ARN
- Regional Domain Name

These outputs are consumed by the CloudFront module.

# ACM Module

## Purpose

The ACM module provisions the SSL/TLS certificate used by Amazon CloudFront.

### Resources Managed

- Certificate Request
- DNS Validation Records
- Certificate Validation

### Responsibilities

- Request a certificate for the custom domain.
- Validate ownership using Route 53.
- Make the certificate available to CloudFront.

### Outputs

- Certificate ARN

The ARN is consumed by the CloudFront module during deployment.

# Route 53 Module

## Purpose

The Route 53 module manages DNS resources for the custom domain.

### Resources Managed

- Hosted Zone
- Alias Record
- Validation Records

### Responsibilities

- Resolve the domain name.
- Validate ACM certificates.
- Route traffic to CloudFront.

### Outputs

- Hosted Zone ID
- Domain Name
- Name Servers

# CloudFront Module

## Purpose

The CloudFront module creates the Content Delivery Network (CDN) used to serve the website globally.

### Resources Managed

- CloudFront Distribution
- Origin Access Control
- Viewer Certificate
- Cache Behavior

### Responsibilities

- Deliver website content globally.
- Cache frequently requested objects.
- Enforce HTTPS.
- Secure communication with Amazon S3.
- Improve website performance.

### Outputs

- Distribution ID
- Distribution Domain Name

These values are later referenced by Route 53.

# Module Dependencies

Although Terraform modules are developed independently, they depend on one another during deployment.

The dependency flow is illustrated below.

```
Amazon S3 Module
        │
        ▼
CloudFront Module
        │
        ▼
Route 53 Module

AWS Certificate Manager
        │
        ▼
CloudFront Module
```

Terraform automatically resolves these dependencies to ensure resources are created in the correct sequence.

# Variables

Terraform variables make the infrastructure reusable by avoiding hardcoded values.

Examples include:

- AWS Region
- Domain Name
- Bucket Name
- Tags
- Environment
- CloudFront Price Class

Variables improve flexibility by allowing the same configuration to be deployed across multiple environments with minimal changes.

# Outputs

Terraform outputs expose important resource information after deployment.

Typical outputs include:

- CloudFront Distribution Domain Name
- S3 Bucket Name
- Certificate ARN
- Hosted Zone ID
- Website URL

These outputs can also be consumed by other Terraform modules or CI/CD pipelines.

# Data Sources

Terraform data sources allow existing AWS resources to be referenced dynamically.

Instead of hardcoding identifiers, Terraform retrieves resource information during deployment.

Examples include:

- Current AWS Region
- Caller Identity
- Existing Hosted Zone
- Managed Cache Policy

Using data sources improves portability and reduces manual configuration.

# State Management

Terraform maintains a state file that tracks all managed infrastructure.

The state file records:

- Existing resources
- Resource IDs
- Dependencies
- Configuration metadata

Terraform compares the desired configuration with the current state to determine which resources need to be created, modified, or destroyed.

This process is known as **Infrastructure Reconciliation**.

# Deployment Lifecycle

The deployment follows a predictable lifecycle.

## Step 1

Terraform initializes the working directory.

```bash
terraform init
```

Providers and modules are downloaded.

## Step 2

Terraform validates the configuration.

```bash
terraform validate
```

Configuration errors are identified before deployment.

## Step 3

Terraform generates an execution plan.

```bash
terraform plan
```

The plan displays the resources that will be created, modified, or removed.

## Step 4

Terraform provisions the infrastructure.

```bash
terraform apply
```

AWS resources are created according to the execution plan.

## Step 5

Terraform stores the infrastructure state for future operations.

# Infrastructure Lifecycle

Terraform continuously manages the lifecycle of the infrastructure.

```
Write Configuration

↓

Validate

↓

Plan

↓

Apply

↓

Infrastructure Created

↓

Modify Configuration

↓

Plan

↓

Apply

↓

Infrastructure Updated
```

This lifecycle enables controlled, predictable, and repeatable infrastructure changes.

# Infrastructure as Code Best Practices

The project follows several Infrastructure as Code best practices.

## Modular Design

Resources are grouped into logical modules to improve maintainability.

## Reusability

Modules can be reused in future projects without significant modifications.

## Version Control

Terraform configuration is stored in Git, enabling collaboration and change tracking.

## Automation

Infrastructure deployment is fully automated, reducing manual effort and human error.

## Consistency

Every deployment produces a predictable infrastructure configuration.

## Reduced Configuration Drift

Infrastructure changes are managed through Terraform rather than manual updates in the AWS Management Console.

## Documentation

Each module is documented to improve maintainability and knowledge transfer.

# Benefits of the Terraform Implementation

Using Terraform for this project provides several operational and architectural advantages.

- Consistent deployments
- Automated infrastructure provisioning
- Infrastructure version control
- Simplified maintenance
- Improved collaboration
- Reusable modules
- Reduced deployment errors
- Faster disaster recovery
- Better scalability
- Easier future enhancements

# Summary

Terraform serves as the foundation of the Akwannya Hub cloud infrastructure.

By adopting a modular Infrastructure as Code approach, the project achieves repeatable deployments, strong maintainability, improved security, and reduced operational complexity.

The modular architecture also provides a scalable foundation that can be extended in the future to support additional AWS services, environments, or automation pipelines without redesigning the existing infrastructure.
