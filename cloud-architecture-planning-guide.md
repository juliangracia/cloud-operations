# Cloud Architecture Planning Guide

## Budget
Questions about budget requirements.

1. What is the spend allowance per month/per year?
1. Is there known budget allocation restrictions? 
    > For instance, are some or all of the allocated budget only to be used for certain services or activities?
1. Is there project or cost segmentation requirements? 
    > For instance, must some deployed artifacts/capabilities be deployed to separate cloud projects?
1. Are there requirements for budget reporting? 
    > For instance, is there a requirement to gain visibility into individual costs per component, service or feature?
1. Cost-to-Availability Tolerance Thresholds. Need to find a way to ask questions about the ROI of scaling of services to handle incremental increases (and on-demand) changes to traffic patterns. 

## Usage
Questions about: 
* system/application use expectations
* system design
* known constraints
* vendor/tool requirements
* external dependencies


1. What is the expected number of users/transactions for the system? *(Might be broken down further by micro service/app)*
1. What types of technologies will be used? *(Languages, Containerization, Databases, Storage, Machine Learning, Big Data, etc.)*
1. Will the system be composed of commercial software (COTS) or all self-developed apps?
1. Which app/services are stateless vs. stateful?
1. Any known hardware/resource requirements for particular app/services?
1. How many environments are needed? *(e.g. Dev, QA, Pre-Prod, Prod, etc.)*
1. Any reason for not being able to use managed services?
1. What managed services are needed?
1. What operating systems are to be used?
1. What kind of ancillary/support systems are needed? *(e.g. artifact repository management, CI/CD, user management/directory, issue tracking, etc.)*
1. How/Who will manage DNS? *(Is there an existing domain?)*
1. What kind of service accounts are needed for the system?
1. Do Cloud resources require access to on-premise resources or visa versa? If so, what are those services and are the method of connectivity identified *(example: on-prem CI/CD pipeline needs to deploy to Cloud location and will do so over VPN or AWS DirectConnect)*
1. Are there any limitations/considerations for automation tools to be used?
1. Are there third-party SaaS products/vendors that need to be integrated? *(Examples: Atlassian-hosted Bitbucket code repo, LaunchDarkly feature management services, etc)*

## Availability

Questions about:
* system high availability
* system deployment


1. What is the minimum level of availability required for your services? *(99.9%, 99.99%, etc.)*
1. Does the system need to be geographically dispersed? What areas?
1. Does the system need to run across multiple public cloud providers? Which ones *(e.g. AWS, GCP, Azure, etc)*?
1. Does data need to be available across regions? *(Replication)*
1. Are there any specific requirements around load balancing? *(Geo-aware, etc.)*
1. Are there any specific Disaster Recovery requirements for all or part of the system? *(Example: Loss of database services must be restored within 6 hours with no more than 12 hours loss of data.)*

## Security
Questions about security requirements.

1. Do you have to comply with any particular security standard? (NIST, PCI, HIPAA, etc)
1. What level of separation is required between environments (e.g. Dev, QA, etc.)? Do they need to be under different projects/accounts, VPCs, etc.?
1. What are the different roles for the people accessing Cloud resources and what access do each of those roles require?
1. Do you require audit logging? (If so, what needs auditing?)
1. Are there any specific requirements around TLS/SSL certificate management?
1. What services will be exposed externally to include connection details?(e.g. Ports, etc.)
1. What are your logging requirements? (e.g. What needs to be logged and what’s the retention policy?)
1. Do you require analytics/monitoring/notification on the logs collected? If so what?

## Compliance
Questions about external regulatory agency compliance. This includes government regulations and third-party certification requirements.

1. Are there U.S. agency regulatory requirements? Agencies could include:  
    - FDA
    - FCC
    - FTC 
    - SEC
    - FDIC
    - DoD
    - Title IX
    - NGO - FINRA (Financial Industry Regulatory Authority)

1. Are there U.S laws that must be considered? Laws could include:
    - HIPAA
    - COPPA - Child Privacy Laws
    - ADA (Section 508)
    - SOX - Sarbanes-Oxley Act of 2002 
    - Dodd-Frank  - Wall Street Reform and Consumer Protection Act
    - Title IX, Education Amendments of 1972
    - State-specific laws (California in particular)

1. Are there third-party requirements/certifications that need to be considered?
    - Credit Card processor requirements
    - Banking requirements

1. Are there non-U.S. laws or agencies that must be considered? This could include:

## Support
Questions about support scenarios.

1. Who will be providing operational support for your system? Scenarios could include any of the following individually or in combination: 
    - Internal support from within the same group.
    - Internal support from another group.
    - External group contracted for this specific support scenario.
    - External group contracted for overall organization support.

1. What are the required support hours?
1. Are there identified support response requirements (e.g. respond within 1 hour, 6 hours or 1 business day, 3 business days, etc.)

## Other Questions
1. What does the development to production process flow look like and who is going to be responsible for the different pieces?
