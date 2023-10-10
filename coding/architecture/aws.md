# AWS Cloud Practitioner Essentials
- Capaian Pembelajaran:
    1. AWS Cloud concepts
    2. AWS services
    3. Security 
    4. Architecture 
    5. Pricing
    6. Support

## Module 1 Introduction
- `Cloud computing` -> On-demand delivery of IT resources and applications through the internet with pay-as-you-go pricing

## Module 2 Compute in The Cloud
### EC2 Instance Types
- `Amazon Elastic Compute Cloud` or `EC2`  is the Amazon Web Service you use to create and run virtual machines in the cloud (we call these virtual machines 'instances'). 
- EC2 types:
    1. General purpose instances
    2. Compute optimized instances
    3. Memory optimized instances
    4. Accelerated computing instances
    5. Storage optimized instances
### EC2 Pricing
- EC2 pricing:
    1. On-Demand
        - No upfront costs or minimum contracts apply. The instances run continuously until you stop them, and you pay for only the compute time you use.
        - Exp:  developing and testing applications and running applications that have unpredictable usage patterns
    2. Reserved Instances
        - There are two available types of Reserved Instances:
            1. Standard Reserved Instances
                - provides a discount when you specify a number of EC2 instances to run a specific OS, instance family and size, and tenancy in one Region
            2. Convertible Reserved Instances
        - You can purchase Standard Reserved and Convertible Reserved Instances for a 1-year or 3-year term
    3. EC2 Instance Savings Plans
        - make an hourly spend commitment to an instance family and Region for a 1-year or 3-year term.
    4. Spot Instances
        -  take advantage of unused EC2 capacity in the AWS cloud 
    5. Dedicated Hosts
        -  are physical servers with Amazon EC2 instance capacity that is fully dedicated to your use. 
### EC2 Scaling
### Directing Traffic with Elastic Load Balancing
- `Elastic Load Balancing` is the AWS service that automatically distributes incoming application traffic across multiple resources, such as Amazon EC2 instances.
### Messaging and Queuing
- `Amazon Simple Queue Service (Amazon SQS)` -> Allow you to send, store, receive messages between software components at any volume
- `Amazon SQS` is a publish/subscribe service. Using Amazon SNS topics, a publisher publishes messages to subscribers. This is similar to the coffee shop; the cashier provides coffee orders to the barista who makes the drinks.
- `Amazon Simple Notification Service (Amazon SNS)` -> is a publish/subscribe service. Using Amazon SNS topics, a publisher publishes messages to subscribers. This is similar to the coffee shop; the cashier provides coffee orders to the barista who makes the drinks.
### Additional Compute Services
- `Serverless computing` -> The term “serverless” means that your code runs on servers, but you do not need to provision or manage these servers. With serverless computing, you can focus more on innovating new products and features instead of maintaining servers.
- `AWS Lambda` is a service that lets you run code without needing to provision or manage servers. 
- `Amazon Elastic Container Service (Amazon ECS)` 
- Container here is Docker container
- `Amazon Elastic Kubernates Service (Amazon EKS)` 
- `AWS Fargate`
- Tips:
    1. If u host traditional applications and want full access to the OS -> use EC2
    2. If u host short running functions, service-oriented applications, event driven applications, and no provisioning or managing servers -> use AWS Lambda    
    3. If u run Docker container-based workloads on AWS -> use ECS/EKS
    4. If u dont want to run ECS/EKS in EC2 instance because u dont want to manage docker container -> use AWS Fargate

## Module 3 Global Infrastructure and Reliability
### AWS Global Infrastructure
- When determining the right `Region` for your services, data, and applications, consider the following four business factors: Compliance, Proximity, Available service within a region, Pricing
- `Avaibility Zone` is a single data center or a group of data centers within a Region
### Edge Locations
- An `edge location` is a site that Amazon CloudFront uses to store cached copies of your content closer to your customers for faster delivery.
- `Amazon Cloudfront`
- `Amazon Route 53`
- `AWS Outposts`
### How to Provision AWS Resources
- Ways to interact with AWS services:
    1. AWS Management Console 
    2. AWS Command Line Interface (AWS CLI)
    3. software development kits (SDKs)
- With `AWS Elastic Beanstalk`, you provide code and configuration settings, and Elastic Beanstalk deploys the resources necessary to perform the following tasks:
    - Adjust capacity
    - Load balancing
    - Automatic scaling
    - Application health monitoring
- With `AWS CloudFormation`, you can treat your infrastructure as code. This means that you can build an environment by writing lines of code instead of using the AWS Management Console to individually provision resources.

## Module 4 Networking
### Connectivity to AWS
- To answer: who should be allowed to communicate with each other?
- `Amazon Virtual Private Cloud (Amazon VPC)` is A networking service that you can use to establish boundaries around your AWS resources
- Amazon VPC enables you to provision an isolated section of the AWS Cloud. In this isolated section, you can launch resources in a virtual network that you define. Within a virtual private cloud (VPC), you can organize your resources into subnets. A subnet is a section of a VPC that can contain resources such as Amazon EC2 instances.
- 3 type of outermost door of VPC:
    1. `Internet gateway`
        - To allow public traffic from the internet to access your VPC, you attach an internet gateway to the VPC.
        ![](img_aws/1.png?raw=true)
            - An internet gateway is a connection between a VPC and the internet. You can think of an internet gateway as being similar to a doorway that customers use to enter the coffee shop. Without an internet gateway, no one can access the resources within your VPC.
    2. `Virtual private gateway`
        - To access private resources in a VPC through internet
        ![](img_aws/2.png?raw=true)
    3. `AWS Direct Connect`
        - is a service that lets you to establish a dedicated private connection between your data center and a VPC.  
        ![](img_aws/3.png?raw=true)
### Subnets and Network Access Control Lists
![](img_aws/4.png?raw=true)
- Different level of network hardening:
    1. Internet Gateway (IGW) di level VPC
        - Public subnet bisa akses internet lewat IGW, private subnet tidak boleh akses langsung IGW, harus lewat NAT pada public subnet
    2. Network Access Control List (Network ACL) ada di level subnet
    3. Security Group (SG) ada di level instances
- `Subnets` -> A subnet is a section of a VPC in which you can group resources based on security or operational needs. Subnets can be public or private. 
- `Network ACLs` -> A network ACL is a virtual firewall that controls inbound and outbound traffic at the subnet level.
- By default, your account’s default network ACL allows all inbound and outbound traffic, but you can modify it by adding your own rules. For custom network ACLs, all inbound and outbound traffic is denied until you add rules to specify which traffic to allow.
- Network ACLs perform `stateless` packet filtering. They remember nothing and check packets that cross the subnet border each way: inbound and outbound. 
- `Security groups` -> A security group is a virtual firewall that controls inbound and outbound traffic for an Amazon EC2 instance.
- By default, a security group denies all inbound traffic and allows all outbound traffic. You can add custom rules to configure which traffic should be allowed; any other traffic would then be denied
- Security groups perform `stateful` packet filtering. They remember previous decisions made for incoming packets.
### Global Networking
- `Domain Name System (DNS)` -> DNS resolution is the process of translating a domain name to an IP address. 
- `Route 53` -> is a DNS web service. It gives developers and businesses a reliable way to route end users to internet applications hosted in AWS. 

## Module 5 Storages and Databases
### Instance Stores and Amazon Elastic Block Store (Amazon EBS)
- An instance store(opens in a new tab) provides temporary block-level storage for an Amazon EC2 instance. An instance store is disk storage that is physically attached to the host computer for an EC2 instance, and therefore has the same lifespan as the instance. When the instance is terminated, you lose any data in the instance store.
- instance store mirip data pada container docker jika tidak pakai volume
- `Amazon Elastic Block Store (Amazon EBS)` is a service that provides block-level storage volumes that you can use with Amazon EC2 instances. If you stop or terminate an Amazon EC2 instance, all the data on the attached EBS volume remains available.
- EBS mirip volume pada container docker
- An `EBS snapshot` is an incremental backup. This means that the first backup taken of a volume copies all the data. For subsequent backups, only the blocks of data that have changed since the most recent snapshot are saved. 
### Amazon Simple Storage Service (Amazon S3)
- `Amazon S3` is a service that provides object-level storage. Amazon S3 stores data as objects in buckets.
- With Amazon S3, you pay only for what you use. You can choose from a range of storage classes to select a fit for your business and cost needs. When selecting an Amazon S3 storage class, consider these two factors:
    1. How often you plan to retrieve your data
    2. How available you need your data to be
- range of storage classes: https://aws.amazon.com/s3/storage-classes/
- Comparing Amazon EBS and Amazon S3
    - EBS ->  block storage -> if there is a change, can only upload block of changes
    - S3 -> object storage -> if there is a change, need to reupload the files    
### Amazon Elastic File System (Amazon EFS)
- `Amazon Elastic File System (Amazon EFS)` is a scalable file system used with AWS Cloud services and on-premises resources. As you add and remove files, Amazon EFS grows and shrinks automatically. It can scale on demand to petabytes without disrupting applications. 
- Comparing Amazon EBS and Amazon EFS
    1. `EBS` volume stores data in a single Availability Zone. 
    2. `EFS` is a regional service. It stores data in and across multiple Availability Zones.
### Amazon Relational Database Service (Amazon RDS)
- `Amazon Relational Database Service (Amazon RDS)` is a service that enables you to run relational databases in the AWS Cloud.
- `Amazon Aurora` is an enterprise-class relational database. It is compatible with MySQL and PostgreSQL relational databases. It is up to five times faster than standard MySQL databases and up to three times faster than standard PostgreSQL databases.
### Amazon DynamoDB
- `Amazon DynamoDB` is a key-value database service. It delivers single-digit millisecond performance at any scale.
### Amazon Redshift
- `Amazon Redshift` is a data warehousing service that you can use for big data analytics. It offers the ability to collect data from many sources and helps you to understand relationships and trends across your data.
### AWS Database Migration Service
- `AWS Database Migration Service (AWS DMS)` enables you to migrate relational databases, nonrelational databases, and other types of data stores.
- With AWS DMS, you move data between a source database and a target database. The source and target databases can be of the same type or different types. During the migration, your source database remains operational, reducing downtime for any applications that rely on the database. 
### Additional Database Services
- `Amazon DocumentDB`
- `Amazon Neptune`
- `Amazon Quantum Ledger Database (Amazon QLDB)`
- `Amazon Managed Blockchain`
- `Amazon ElastiCache`
- `Amazon DynamoDB Accelerator`

## Module 6 Security
### AWS Shared Responsibility Model
- https://aws.amazon.com/compliance/shared-responsibility-model/
### User Permissions and Access
- `AWS Identity and Access Management (IAM)` enables you to manage access to AWS services and resources securely.   
- IAM gives you the flexibility to configure access based on your company’s specific operational and security needs. You do this by using a combination of IAM features, which are explored in detail in this lesson:
    - IAM users, groups, and roles
    - IAM policies
    - Multi-factor authentication
- `AWS account root user`, `IAM users`, `IAM policies`, `IAM groups`, `IAM roles`, `Multi-factor authentication` details in: https://explore.skillbuilder.aws/learn/course/134/play/85854/aws-cloud-practitioner-essentials
### AWS Organizations
- `AWS Organizations`, `Organizational units (OU)` -> https://explore.skillbuilder.aws/learn/course/134/play/85854/aws-cloud-practitioner-essentials
### Compliance
- `AWS Artifact` is a service that provides on-demand access to AWS security and compliance reports and select online agreements. AWS Artifact consists of two main sections: AWS Artifact Agreements and AWS Artifact Reports.
- `The Customer Compliance Center` contains resources to help you learn more about AWS compliance. In the Customer Compliance Center, you can read customer compliance stories to discover how companies in regulated industries have solved various compliance, governance, and audit challenges.
### Denial-of-Service Attacks
- A `denial-of-service (DoS)` attack is a deliberate attempt to make a website or application unavailable to users.
- `AWS Shield` is a service that protects applications against DDoS attacks. AWS Shield provides two levels of protection: Standard and Advanced.
### Additional Security Services
- `AWS Key Management Service (AWS KMS)` -> enables you to perform encryption operations through the use of cryptographic keys. A cryptographic key is a random string of digits used for locking (encrypting) and unlocking (decrypting) data. You can use AWS KMS to create, manage, and use cryptographic keys. You can also control the use of keys across a wide range of services and in your applications.
- `AWS WAF` is a web application firewall that lets you monitor network requests that come into your web applications. 
- `Amazon Inspector`
- `Amazon GuardDuty`

## Module 7 Monitoring and Analytics
### Amazon CloudWatch
- `Amazon CloudWatch` is a web service that enables you to monitor and manage various metrics and configure alarm actions based on data from those metrics.
- `CloudWatch alarms` you can create alarms that automatically perform actions if the value of your metric has gone above or below a predefined threshold. 
### AWS CloudTrail
- `AWS CloudTrail` records API calls for your account. The recorded information includes the identity of the API caller, the time of the API call, the source IP address of the API caller, and more. You can think of CloudTrail as a “trail” of breadcrumbs (or a log of actions) that someone has left behind them.
### AWS Trusted Advisor
- `AWS Trusted Advisor` is a web service that inspects your AWS environment and provides real-time recommendations in accordance with AWS best practices.
- usted Advisor compares its findings to AWS best practices in five categories: cost optimization, performance, security, fault tolerance, and service limits.

## Module 8 Pricing and Support
### AWS Free Tier
- The `AWS Free Tier` enables you to begin using certain services without having to worry about incurring costs for the specified period. 
- Three types of offers are available: 
    1. Always Free
    2. 12 Months Free
    3. Trials
### AWS Pricing Concepts
- https://explore.skillbuilder.aws/learn/course/134/play/85854/aws-cloud-practitioner-essentials
- `The AWS Pricing Calculator` lets you explore AWS services and create an estimate for the cost of your use cases on AWS. You can organize your AWS estimates by groups that you define. A group can reflect how your company is organized, such as providing estimates by cost center.
### Billing Dashboard
- Use the `AWS Billing & Cost Management` dashboard to pay your AWS bill, monitor your usage, and analyze and control your costs.
### Consolidated Billing
- https://explore.skillbuilder.aws/learn/course/134/play/85854/aws-cloud-practitioner-essentials
### AWS Budgets
- In AWS Budgets, you can create budgets to plan your service usage, service costs, and instance reservations. The information in AWS Budgets updates three times a day. This helps you to accurately determine how close your usage is to your budgeted amounts or to the AWS Free Tier limits. In AWS Budgets, you can also set custom alerts when your usage exceeds (or is forecasted to exceed) the budgeted amount.
### AWS Cost Explorer
### AWS Support Plans
- AWS offers four different `Support plans` to help you troubleshoot issues, lower costs, and efficiently use AWS services. 
- You can choose from the following Support plans to meet your company’s needs: 
    - Basic
    - Developer
    - Business
    - Enterprise On-Ramp
    - Enterprise
- The Enterprise On-Ramp and Enterprise Support plans include access to a `Technical Account Manager (TAM)`
- The TAM is your primary point of contact at AWS
### AWS Marketplace
- `AWS Marketplace` is a digital catalog that includes thousands of software listings from independent software vendors. You can use AWS Marketplace to find, test, and buy software that runs on AWS. 
- AWS Marketplace offers products in several categories, such as Infrastructure Software, DevOps, Data Products, Professional Services, Business Applications, Machine Learning, Industries, and Internet of Things (IoT).

## Module 9 Migration and Innovation
### AWS Cloud Adoption Framework (AWS CAF)
- At the highest level, the `AWS Cloud Adoption Framework (AWS CAF)` organizes guidance into six areas of focus, called Perspectives. Each Perspective addresses distinct responsibilities. The planning process helps the right people across the organization prepare for the changes ahead.
- https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf
- In general, the `Business, People, and Governance Perspectives` focus on business capabilities, whereas the `Platform, Security, and Operations` Perspectives focus on technical capabilities.
### Migration Strategies
- 6 strategies for migration:
    - Rehosting
    - Replatforming
    - Refactoring/re-architecting
    - Repurchasing
    - Retaining
    - Retiring
- https://aws.amazon.com/blogs/enterprise-strategy/6-strategies-for-migrating-applications-to-the-cloud/
### AWS Snow Family
- The `AWS Snow Family` is a collection of physical devices that help to physically transport up to exabytes of data into and out of AWS. 
- AWS Snow Family is composed of `AWS Snowcone, AWS Snowball, and AWS Snowmobile`. 
### Innovation with AWS
- https://explore.skillbuilder.aws/learn/course/134/play/85854/aws-cloud-practitioner-essentials

## Module 10 The Cloud Journey
### The AWS Well-Architected Framework
- The `AWS Well-Architected Framework` helps you understand how to design and operate reliable, secure, efficient, and cost-effective systems in the AWS Cloud. It provides a way for you to consistently measure your architecture against best practices and design principles and identify areas for improvement.
- https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html
- The Well-Architected Framework is based on six pillars: 
    - Operational excellence
    - Security
    - Reliability
    - Performance efficiency
    - Cost optimization
    - Sustainability
### Benefits of the AWS Cloud

