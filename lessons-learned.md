# Lessons Learned

## Introduction

Developing the **Akwannya Hub Static Website Infrastructure on AWS** provided valuable hands-on experience in designing, deploying, securing, and documenting cloud infrastructure using Infrastructure as Code (IaC).

Beyond learning how to provision AWS resources, this project reinforced the importance of planning, automation, security, validation, and operational best practices. It also demonstrated how multiple AWS managed services can work together to deliver a secure, scalable, and highly available static website.

This document summarizes the key technical, operational, and architectural lessons learned throughout the project.

# Technical Lessons

## Infrastructure as Code Improves Consistency

Using Terraform to provision AWS resources eliminated the need for repetitive manual configuration.

Benefits observed included:

- Repeatable deployments
- Reduced human error
- Version-controlled infrastructure
- Simplified maintenance
- Easier collaboration

Infrastructure as Code proved to be more reliable and maintainable than manual provisioning through the AWS Management Console.

## Modular Terraform Design Simplifies Management

Organizing Terraform configurations into reusable modules improved the overall project structure.

Benefits included:

- Better organization
- Improved readability
- Easier troubleshooting
- Code reuse
- Simpler future enhancements

This reinforced the importance of modular design for medium and large infrastructure projects.


## Managed AWS Services Reduce Operational Overhead

Using AWS managed services significantly reduced the operational burden of managing infrastructure.

Examples include:

- Amazon CloudFront
- Amazon Route 53
- AWS Certificate Manager
- Amazon S3

These services automatically provide features such as scalability, availability, and maintenance, allowing more focus on application delivery rather than infrastructure management.

# Security Lessons

## Private Storage Is More Secure Than Public Website Hosting

Initially, it may seem simpler to host a static website directly from an Amazon S3 bucket.

However, using a private S3 bucket with Amazon CloudFront and Origin Access Control provides a much stronger security posture.

This approach:

- Reduces the attack surface.
- Prevents direct object access.
- Centralizes content delivery.
- Aligns with current AWS recommendations.

## Defense in Depth Is Essential

No single security control is sufficient on its own.

This project demonstrated how multiple controls—including HTTPS, TLS, Origin Access Control, Block Public Access, Bucket Policies, Server-Side Encryption, and IAM—work together to provide layered protection.

This reinforced the importance of designing security as a combination of complementary controls.

## Least Privilege Improves Security

Granting only the permissions required for users and services reduces the risk of unauthorized access and accidental changes.

Applying the Principle of Least Privilege helped reinforce secure cloud administration practices.

# Networking Lessons

## DNS Is a Critical Component

Amazon Route 53 does more than map a domain name to an IP address.

It also supports:

- Alias records
- Domain validation
- Integration with CloudFront
- High availability

Understanding DNS behavior proved essential for successful deployment.

## HTTPS Is More Than Encryption

Implementing HTTPS improves:

- Confidentiality
- Integrity
- Authentication
- Browser trust
- User confidence

The project highlighted the importance of properly configuring SSL/TLS certificates and secure communication.

# Deployment Lessons

## Validation Prevents Deployment Issues

Performing validation after deployment is just as important as deploying the infrastructure itself.

Systematically verifying:

- HTTPS
- CloudFront
- Route 53
- Amazon S3
- DNS
- Terraform outputs

helped confirm that the environment matched the intended architecture.

## Documentation Is Part of the Project

Producing detailed documentation required almost as much effort as building the infrastructure.

Comprehensive documentation makes it easier to:

- Understand the architecture.
- Reproduce the deployment.
- Troubleshoot issues.
- Transfer knowledge.
- Maintain the environment.

This project reinforced that documentation is an important deliverable, not an afterthought.

# Operational Lessons

## Monitoring Should Be Considered Early

Although this project uses a relatively simple architecture, monitoring remains important.

Planning for monitoring during the design phase makes future troubleshooting and operational management easier.

## Troubleshooting Benefits from a Structured Approach

Following a consistent troubleshooting process—identifying the affected service, reviewing configurations, and validating changes—helps reduce downtime and resolve issues more efficiently.

# Challenges Encountered

Several challenges were encountered during the project, including:

- Understanding the interactions between CloudFront, Amazon S3, and Origin Access Control.
- Configuring DNS validation for the SSL/TLS certificate.
- Waiting for CloudFront deployments and DNS propagation.
- Structuring reusable Terraform modules.
- Ensuring security configurations aligned with AWS best practices.

Working through these challenges improved both technical knowledge and problem-solving skills.

# Future Improvements

If the project were extended further, potential enhancements could include:

- AWS WAF for web application protection.
- AWS CloudTrail for API auditing.
- Amazon GuardDuty for threat detection.
- AWS Config for configuration compliance.
- AWS Security Hub for centralized security management.
- CloudWatch alarms with automated notifications.
- CI/CD pipeline using GitHub Actions.
- Automated Terraform validation and formatting.

These enhancements would further strengthen the project's security, observability, and operational maturity.

# Professional Growth

Completing this project strengthened practical experience in:

- AWS cloud infrastructure
- Infrastructure as Code (Terraform)
- Cloud networking
- DNS management
- Content delivery networks
- Cloud security
- Technical documentation
- Cloud operations
- Solution architecture

The project also reinforced the value of planning, automation, and secure-by-design principles when building cloud solutions.


# Key Takeaways

The most significant lessons from this project include:

- Infrastructure as Code simplifies cloud deployments.
- Security should be incorporated into the architecture from the beginning.
- Managed AWS services provide operational advantages.
- Comprehensive documentation improves maintainability.
- Validation and monitoring are essential components of successful deployments.
- Well-designed cloud architectures balance security, scalability, availability, and operational simplicity.


# Conclusion

The Akwannya Hub Static Website Infrastructure project provided practical experience across multiple areas of AWS cloud engineering, including networking, security, automation, deployment, monitoring, and documentation.

By combining Terraform with AWS managed services, the project demonstrated how modern cloud infrastructure can be deployed consistently, secured using AWS best practices, and documented for long-term maintainability.

The knowledge and experience gained through this project establish a solid foundation for designing and operating secure, scalable, and production-ready cloud solutions.
