# Request Flow

## Overview

This document describes how requests are processed within the Akwannya Hub static website architecture.

Understanding the request lifecycle helps explain how AWS managed services collaborate to deliver secure, reliable, and high-performance content to users while ensuring that the origin storage remains private.

Unlike a traditional web application where users communicate directly with a web server, this architecture uses Amazon CloudFront as the only public entry point into the environment.

Amazon S3 never receives requests directly from website visitors.

# Request Flow Diagram

```
Website Visitor
       │
       │ HTTPS Request
       ▼
Amazon Route 53
       │
       ▼
Amazon CloudFront
       │
       │ Cache Lookup
       │
       ├──────────────► Cache Hit
       │                    │
       │                    ▼
       │              Return Content
       │
       ▼
Origin Access Control (OAC)
       │
       ▼
Private Amazon S3 Bucket
       │
       ▼
Return Website Files
       │
       ▼
CloudFront Cache
       │
       ▼
Website Visitor
```

# Step 1 — User Requests the Website

A visitor opens a web browser and enters the Akwannya Hub website URL.

Example:

```
https://www.akwannyahub.com
```

The browser first needs to determine where the website is hosted before it can establish a connection.

# Step 2 — DNS Resolution

The browser sends a DNS query to Amazon Route 53.

Route 53 is responsible for resolving the domain name into the appropriate CloudFront distribution.

Rather than returning an IP address for an EC2 instance or web server, Route 53 returns an Alias Record pointing directly to Amazon CloudFront.

This enables users to access the website using a friendly domain name while allowing AWS to manage the underlying infrastructure.

# Step 3 — Secure HTTPS Connection

After the CloudFront distribution has been resolved, the browser establishes a secure HTTPS connection.

CloudFront presents the SSL/TLS certificate that was issued through AWS Certificate Manager (ACM).

During this process:

- The certificate is validated.
- An encrypted TLS session is established.
- All communication between the client and CloudFront becomes encrypted.

This protects user data from interception or tampering while in transit.

# Step 4 — CloudFront Cache Lookup

CloudFront checks whether the requested object already exists in one of its edge locations.

Examples of cached objects include:

- HTML pages
- CSS files
- JavaScript files
- Images
- Icons
- Fonts
- PDF documents

Two possible scenarios occur.

## Scenario A — Cache Hit

If the requested object already exists in the CloudFront cache:

- No request is sent to Amazon S3.
- CloudFront immediately returns the cached object.
- Response time is significantly reduced.
- The load on the origin bucket is minimized.

This is the ideal scenario because it improves performance while reducing operating costs.

## Scenario B — Cache Miss

If the requested object is not available in the cache:

CloudFront forwards the request to the origin.

Unlike many traditional architectures, the origin is **not publicly accessible**.

Instead, CloudFront must authenticate itself before Amazon S3 will return any objects.

# Step 5 — Origin Access Control (OAC)

CloudFront uses Origin Access Control (OAC) to securely communicate with Amazon S3.

Origin Access Control signs each request sent from CloudFront to the S3 bucket.

Amazon S3 validates the request against the bucket policy.

If the request originates from the authorized CloudFront distribution, access is granted.

If the request originates from any other source, access is denied.

This ensures that website content cannot be retrieved directly from Amazon S3.

# Step 6 — Amazon S3 Processes the Request

Once authentication succeeds, Amazon S3 locates the requested object.

Examples include:

- index.html
- about.html
- style.css
- logo.png
- app.js

Before returning the object, Amazon S3 applies the configured security controls.

These include:

- Block Public Access
- Bucket Policy
- Server-Side Encryption
- Versioning

The object is then returned securely to CloudFront.

# Step 7 — CloudFront Stores the Object

CloudFront receives the requested object from Amazon S3.

The object is stored temporarily within the nearest edge location according to the configured cache behavior.

Future requests for the same object can now be served directly from CloudFront without contacting Amazon S3 again.

This process:

- Improves performance
- Reduces latency
- Minimizes S3 requests
- Reduces bandwidth consumption
- Lowers AWS operating costs

# Step 8 — Content Delivered to the User

CloudFront sends the requested content back to the browser over the encrypted HTTPS connection.

The browser downloads the HTML page.

Additional assets such as CSS, JavaScript, images, and fonts are requested using the same workflow.

The user experiences:

- Fast page loading
- Secure HTTPS communication
- Low latency
- High availability

# Infrastructure Provisioning Flow

The runtime request flow described above is made possible through Infrastructure as Code using Terraform.

Terraform provisions the AWS environment in the following sequence:

1. Amazon S3 bucket
2. Bucket Versioning
3. Server-Side Encryption
4. Block Public Access
5. Bucket Policy
6. AWS Certificate Manager certificate
7. DNS validation records
8. CloudFront distribution
9. Origin Access Control
10. Route 53 Alias Record

Once these resources are deployed, Terraform records their state to ensure future deployments remain consistent and repeatable.

# Security Throughout the Request Lifecycle

Security is applied at every stage of the request flow.

| Stage | Security Control |
|--------|------------------|
| Browser → CloudFront | HTTPS (TLS encryption) |
| Route 53 | Managed DNS service |
| CloudFront | Viewer Certificate (ACM) |
| CloudFront → S3 | Origin Access Control (OAC) |
| Amazon S3 | Private Bucket |
| Amazon S3 | Block Public Access |
| Amazon S3 | Bucket Policy |
| Amazon S3 | Server-Side Encryption |
| Amazon S3 | Versioning |

These controls ensure that website content is delivered securely while protecting the origin from unauthorized access.

# Performance Optimizations

Several AWS services work together to improve website performance.

## Amazon CloudFront

Caches frequently requested objects at edge locations close to users.

## Route 53

Provides low-latency DNS resolution through AWS's globally distributed DNS infrastructure.

## Amazon S3

Delivers highly durable and scalable object storage without requiring server management.

## Origin Access Control

Provides secure access to the origin without exposing Amazon S3 publicly.

# Summary

The request lifecycle demonstrates how AWS managed services collaborate to deliver secure and high-performance static website hosting.

Instead of relying on traditional web servers, the architecture uses Amazon Route 53, CloudFront, Origin Access Control, and Amazon S3 to create a modern, cloud-native solution.

This approach provides:

- Secure HTTPS communication
- Private origin storage
- Low-latency content delivery
- Automatic scalability
- Reduced operational overhead
- Improved cost efficiency
- Strong security through defense-in-depth
