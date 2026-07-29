# Monitoring

## Introduction

Monitoring is an essential component of cloud infrastructure management. It provides visibility into the health, availability, performance, and operational status of deployed resources.

For the Akwannya Hub static website architecture, monitoring helps ensure that the website remains available, performs efficiently, and continues to operate according to design expectations.

Although this project hosts a static website, monitoring remains important for identifying deployment issues, diagnosing performance problems, and supporting future operational improvements.

# Monitoring Objectives

The monitoring strategy was designed to:

- Verify infrastructure health.
- Monitor website availability.
- Observe CloudFront performance.
- Detect operational issues.
- Support troubleshooting.
- Improve reliability.
- Provide operational visibility.

# AWS Monitoring Services

The project leverages several AWS services that provide operational insights.

| AWS Service | Purpose |
|--------------|---------|
| Amazon CloudWatch | Metrics, dashboards, and alarms |
| Amazon CloudFront | Content delivery metrics |
| Amazon Route 53 | DNS health and routing |
| Amazon S3 | Storage metrics |
| AWS Health Dashboard | AWS service health notifications |

# Monitoring Architecture

```
Website Visitors

        │

        ▼

Amazon CloudFront
        │
        │ Metrics
        ▼
Amazon CloudWatch

        │
        ▼

Operational Visibility
```

CloudWatch acts as the primary monitoring service by collecting and displaying operational metrics from AWS services.

# CloudFront Monitoring

Amazon CloudFront provides several useful metrics for evaluating content delivery performance.

Typical metrics include:

- Requests
- Bytes Downloaded
- Bytes Uploaded
- Error Rate
- Cache Hit Ratio
- Total Requests

Monitoring these metrics helps determine whether the content delivery network is operating efficiently.


# CloudFront Health Indicators

The following indicators should be monitored regularly.

| Metric | Purpose |
|---------|---------|
| Requests | Measures website traffic |
| Error Rate | Detects failed requests |
| Cache Hit Ratio | Indicates cache efficiency |
| Data Transfer | Monitors bandwidth usage |
| Total Requests | Measures overall demand |

Unexpected changes may indicate configuration issues or increased traffic.

# Amazon S3 Monitoring

Although website visitors access content through CloudFront, Amazon S3 remains the origin storage service.

Useful monitoring areas include:

- Bucket size
- Object count
- Storage growth
- Access requests
- Server-side encryption status

Monitoring storage usage helps manage costs and capacity over time.

# Route 53 Monitoring

Route 53 plays a critical role in directing users to the CloudFront distribution.

Monitoring considerations include:

- DNS availability
- Domain resolution
- Hosted Zone configuration
- Alias record health

Correct DNS configuration ensures users can consistently reach the website.

# Website Availability

The website should be periodically validated to ensure:

- Homepage loads successfully.
- Images load correctly.
- CSS files are accessible.
- JavaScript files function correctly.
- HTTPS certificate remains valid.
- Custom domain resolves successfully.

These checks help confirm that the website is functioning from an end-user perspective.


# CloudWatch Metrics

Amazon CloudWatch collects operational metrics for supported AWS services.

Examples include:

| Resource | Metric |
|----------|--------|
| CloudFront | Requests |
| CloudFront | Error Rate |
| CloudFront | Data Transfer |
| Amazon S3 | Bucket Size |
| Amazon S3 | Object Count |

These metrics provide insight into infrastructure health and usage.


# CloudWatch Dashboards

CloudWatch Dashboards can be created to display key metrics in a single view.

A dashboard for this project could include:

- CloudFront Requests
- CloudFront Error Rate
- Cache Hit Ratio
- S3 Bucket Size
- Website Availability

Dashboards simplify operational monitoring by presenting important metrics together.

# CloudWatch Alarms

CloudWatch Alarms can notify administrators when predefined thresholds are exceeded.

Examples include:

| Alarm | Purpose |
|--------|---------|
| High Error Rate | Detect excessive failed requests |
| Low Cache Hit Ratio | Identify caching issues |
| Unusual Traffic Increase | Detect abnormal activity |
| Storage Growth | Monitor increasing storage usage |

Alarms support proactive monitoring by highlighting potential issues before they significantly impact users.

# Operational Monitoring Checklist

Routine monitoring should include:

- CloudFront distribution status
- Website accessibility
- HTTPS certificate validity
- DNS resolution
- Amazon S3 storage usage
- Terraform state consistency
- CloudWatch metrics
- CloudWatch alarms

# Performance Monitoring

Performance monitoring focuses on ensuring a fast user experience.

Key performance indicators include:

- Response time
- Cache efficiency
- Global content delivery
- HTTPS availability
- Website responsiveness

CloudFront edge locations help reduce latency for users around the world.

# Security Monitoring Considerations

While this project does not deploy dedicated security monitoring services, administrators should routinely verify:

- Block Public Access remains enabled.
- Bucket policies have not changed unexpectedly.
- Origin Access Control is still configured.
- HTTPS is enforced.
- TLS certificates remain valid.
- IAM permissions continue to follow the Principle of Least Privilege.

Regular reviews help maintain the intended security posture.

# Incident Response

If an issue is detected:

1. Identify the affected AWS resource.
2. Review CloudWatch metrics.
3. Confirm CloudFront distribution status.
4. Verify Route 53 DNS configuration.
5. Check Amazon S3 bucket configuration.
6. Review recent Terraform changes.
7. Apply corrective actions.
8. Validate that the issue has been resolved.

A structured response process helps minimize downtime and supports faster recovery.

# Future Monitoring Enhancements

As the environment grows, additional AWS monitoring capabilities could be introduced, including:

- Amazon CloudWatch Synthetics
- AWS CloudTrail
- Amazon EventBridge
- AWS Config
- AWS Security Hub
- Amazon GuardDuty
- AWS X-Ray (for dynamic applications)

These services would provide deeper operational and security visibility.

# Monitoring Summary

Monitoring provides continuous visibility into the health and performance of the Akwannya Hub static website infrastructure.

By leveraging Amazon CloudWatch, CloudFront metrics, Route 53, and Amazon S3 monitoring capabilities, administrators can observe infrastructure behavior, identify operational issues, and maintain a reliable user experience.

Although the current architecture is intentionally simple, the monitoring approach establishes a strong operational foundation that can scale as the solution evolves.
