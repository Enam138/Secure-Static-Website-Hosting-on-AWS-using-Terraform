# Common Issues and Troubleshooting

## Introduction

Even with Infrastructure as Code and managed AWS services, deployment and operational issues can occur due to configuration errors, propagation delays, permission problems, or DNS settings.

This document outlines common issues that may be encountered while deploying or maintaining the Akwannya Hub static website infrastructure and provides recommended troubleshooting steps.


# Troubleshooting Workflow

When an issue occurs, use the following process:

```
Identify the Problem

        │

        ▼

Review Error Messages

        │

        ▼

Determine Affected AWS Service

        │

        ▼

Verify Configuration

        │

        ▼

Apply Corrective Action

        │

        ▼

Validate Resolution
```

This structured approach helps reduce downtime and simplifies problem resolution.

# Issue 1 – Terraform Initialization Fails

## Symptoms

- `terraform init` returns an error.
- Required providers cannot be downloaded.
- Backend initialization fails.

## Possible Causes

- No internet connection.
- Incorrect Terraform version.
- Invalid provider configuration.

## Resolution

- Verify internet connectivity.
- Confirm the installed Terraform version.
- Check the `providers.tf` configuration.
- Run:

```bash
terraform init -upgrade
```

# Issue 2 – Terraform Validation Errors

## Symptoms

```bash
terraform validate
```

returns errors.

## Possible Causes

- Syntax errors.
- Missing variables.
- Incorrect resource references.
- Module configuration issues.

## Resolution

- Review the reported line numbers.
- Verify variable definitions.
- Ensure referenced modules exist.
- Re-run:

```bash
terraform validate
```

# Issue 3 – Terraform Apply Fails

## Symptoms

- Deployment stops before completion.
- AWS resources are not created.

## Possible Causes

- IAM permission issues.
- Invalid AWS credentials.
- Resource naming conflicts.
- Service quotas reached.

## Resolution

- Verify AWS credentials:

```bash
aws sts get-caller-identity
```

- Review Terraform output.
- Confirm IAM permissions.
- Resolve resource conflicts.
- Retry:

```bash
terraform apply
```

# Issue 4 – Website Displays "Access Denied"

## Symptoms

The browser returns:

```
Access Denied
```

## Possible Causes

- Incorrect bucket policy.
- Origin Access Control not configured.
- CloudFront distribution cannot access Amazon S3.

## Resolution

Verify:

- Bucket policy.
- Origin Access Control configuration.
- CloudFront origin settings.
- Amazon S3 Block Public Access configuration.

# Issue 5 – Website Not Accessible

## Symptoms

The website does not load using the custom domain.

## Possible Causes

- DNS propagation.
- Route 53 configuration.
- CloudFront deployment still in progress.

## Resolution

Check:

- Route 53 Hosted Zone.
- Alias record configuration.
- CloudFront deployment status.
- DNS propagation using:

```bash
nslookup www.akwannyahub.com
```

or

```bash
dig www.akwannyahub.com
```

# Issue 6 – HTTPS Certificate Errors

## Symptoms

The browser displays certificate warnings.

## Possible Causes

- ACM certificate not validated.
- Incorrect certificate attached to CloudFront.
- DNS validation incomplete.

## Resolution

Verify:

- ACM certificate status.
- DNS validation records.
- CloudFront viewer certificate configuration.

Wait for certificate issuance if validation has recently been completed.

# Issue 7 – CloudFront Changes Not Visible

## Symptoms

Recent updates are not reflected on the website.

## Possible Causes

- Cached CloudFront objects.
- Browser cache.
- Distribution deployment still in progress.

## Resolution

- Wait for CloudFront deployment to complete.
- Clear browser cache.
- Create a CloudFront invalidation if immediate updates are required.

# Issue 8 – Direct Amazon S3 Access Fails

## Symptoms

Opening the S3 object URL returns:

```
Access Denied
```

## Expected Behavior

This is the intended behavior.

The Amazon S3 bucket is private and accepts requests only from the configured CloudFront distribution through Origin Access Control.

No action is required.


# Issue 9 – DNS Changes Not Propagated

## Symptoms

Some users can access the website while others cannot.

## Possible Causes

- DNS propagation delay.
- Local DNS cache.

## Resolution

- Allow time for global DNS propagation.
- Flush the local DNS cache if necessary.
- Confirm Route 53 records are correct.

# Issue 10 – Website Assets Missing

## Symptoms

Images, CSS, or JavaScript files fail to load.

## Possible Causes

- Files not uploaded.
- Incorrect object paths.
- Missing references in HTML.

## Resolution

Verify:

- Objects exist in Amazon S3.
- File names match exactly.
- HTML references are correct.
- CloudFront cache has refreshed.

# Issue 11 – Permission Denied During Deployment

## Symptoms

Terraform reports authorization errors.

Example:

```
AccessDenied
```

## Possible Causes

- Missing IAM permissions.
- Expired credentials.
- Incorrect AWS profile.

## Resolution

- Confirm the active AWS identity.
- Review IAM permissions.
- Refresh credentials if necessary.
- Retry the deployment.


# Issue 12 – Slow Website Performance

## Symptoms

Website loads more slowly than expected.

## Possible Causes

- CloudFront cache warming.
- Large static assets.
- Network latency.

## Resolution

- Wait for CloudFront caching to stabilize.
- Optimize images and static files.
- Enable compression where appropriate.
- Monitor CloudFront metrics.


# General Troubleshooting Checklist

When investigating issues, verify:

- Terraform configuration.
- AWS CLI authentication.
- CloudFront distribution status.
- Route 53 DNS records.
- ACM certificate status.
- Amazon S3 bucket configuration.
- Bucket policy.
- Origin Access Control.
- Website object uploads.
- HTTPS functionality.


# Diagnostic Commands

Useful commands during troubleshooting include:

### Verify AWS Identity

```bash
aws sts get-caller-identity
```

### Initialize Terraform

```bash
terraform init
```

### Validate Configuration

```bash
terraform validate
```

### Review Planned Changes

```bash
terraform plan
```

### Apply Infrastructure

```bash
terraform apply
```

### Destroy Infrastructure

```bash
terraform destroy
```

### Check DNS Resolution

```bash
nslookup www.akwannyahub.com
```

or

```bash
dig www.akwannyahub.com
```

# Escalation Guidance

If the issue cannot be resolved through the steps above:

1. Review Terraform output logs.
2. Review AWS service status.
3. Check CloudWatch metrics.
4. Verify recent infrastructure changes.
5. Compare the deployed environment with the Terraform configuration.
6. Consult AWS documentation for the affected service.


# Summary

Most deployment and operational issues can be resolved by systematically reviewing Terraform configuration, AWS resource settings, IAM permissions, and DNS configuration.

By following the troubleshooting procedures in this document, administrators can quickly identify the root cause of common issues and restore normal operation while maintaining the security and reliability of the Akwannya Hub static website infrastructure.
