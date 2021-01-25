---
layout: post
title: "???????????????"
comments: true
categories: Other
---

IAM

- IAM is universal. It does not apply to regions at this time.
- The "root account" is simply the account created when first setting your AWS account. It has complete Admin access
- New Users have NO permissions when first created
- New Users are assigned Access Key ID & Secret Access Keys when first created
- These are not the same as a password. You cannot use the Access key ID & Secret Access Key to Log into the console. You can use this to access AWS via the APIs and Comman Line, however.
- You only get to view these once. If you lose them, you have to regenerate them. So, save them in a secure location

- Always setup Multifactor Authentication on your root account
- You can create and customise your own password rotation policies.

Aws
Automatic notifications : Cloud Watch → Billing Alarm(using SNS topic)

S3

- S3 is Object-based: i.e. allows you to upload files
- Files can be from 0 bytes to 5tb
- There is unlimited storage
- Files are stored in Buckets
- S3 is a universal namespace. That is, names must be unique globally
- Not suitable to install an OS on
- Successful uploads will generate a HTTP 200 status code.
- You can turn on MFA DELETE
- Read after Write consistency for PUTS of new objects
- Eventual Consistency for overwrite PUTS and DELETES(can take some time to propagate)
- Bucket names share a common name space. You cannot have the same bucket name as someone else
- [When you view your buckets you view them globally] but you can have buckets in individual regions
- You can replicate the contents of one bucket to another bucket automatically by using CROSS REGION REPLICATION
- You can change STORAGE CLASSES and ENCRYPTION of your objects on the fly

Restricting bucket Access

- Bucket Policies – applies across the whole bucket
- Object Policies – applies to individual files
- IAM Policies to Users & Groups – applies to Users & Groups

S3 classes

- S3 Standard: 99.99 availability, 11 99 durability
- S3-IA : Infrequently Acccessed, requires rapid access when needed, lower fee than S3, charged a retrieval fee
- S3 One Zone – IA : RRS
- S3 – Interlligent Tiering
- S3 Glacier : retrieval time from minutes to hours
- S3 Glacier Deep Archive : cheapest, retrieval time 12 hours

* Amazon Glacier: an aws service designed for long term data archival

Key Fundamentals of S3

- Key, value (simply the data and is made up of a sequence of bytes)
- Version ID
- Metadata
- Subresources: Access Control List, torrent

S3 Security And Encryption

- Encryption IN TRANSIT is Achieved by : SSL/TLS
- Encryption At REST (SERVER SIDE) is Achieved by:
   S3 Managed keys – SSE-S3
   AWS Key Management Service, Managed Keys – SSE-KMS
   Server Side Encryption With Customer Provided Keys – SSE-C
- CLIENT SIDE Encryption

S3 Version Control

- Stores all versions of an object (including all writes and even if you delete an object)
- Great backup tool
- Once enables, versioning cannot be disabled, only suspended
- Integrates with Life cycle rules
- Versioning’s MFA Delete capability, which uses multi-factor authentication, can be used to provide an additional layer of security

S3 Lifecycle management

- Automates moving your objects between the different storage tiers.
- Can be used in conjunction with versioning
- Can be applied to current versions and previous versions

S3 Lock Policies & Glacier Vault Lock

- Use S3 Object Lock to store objects using a write once, read many (WORM) model (prevent objects from being deleted or modified for a fixed amount of time or indefinitely.)
- Object locks can be on individual objects or applied across the bucket as a whole
- Objects locks come in two modes: governance mode and compliance mode.
  - With GOVERNANCE mode, users can’t overwrite or delete an object version or alter its lock settings UNLESS THEY HAVE SPECIAL PERMISSIONS.
  - With COMPLIANCE mode, a protected object version can’t be overwritten or deleted by any user, including the root user in your AWS account DURING RETENTION PERIODS.
- Retention Period protects an object version for a fixed amount of time
- A LEGAL HOLD prevents an object version from being overwritten or deleted
- S3 Glacier Vault Lock allows you to easily deploy and enforce compliance controls for individual S3 Glacier vaults with a Vault Lock policy. You can specify controls such as WORM in a Vault Lock policy and lock the policy from future edits. ONCE LOCKED, THE POLICY CAN NO LONGER BE CHANGED.

S3 Performance

- Prefixes : mybucketname/folder1/subfolder1/myfile.jpg → /folder1/subfolder1
- The more prefixes you use, the better performance you get out of S3
- You can also achieve a high number of requests : 3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second per prefix
- You can get better performance by spreading your reads across different prefixes. For example, if you are using two prefixes, you can achieve 11,000 requests per second.
- S3 limitations when using SSE-KMS to encrypt you objects in S3
   Uploading/downloading will count toward the KMS quota
   Region-specific, however, it’s either 5,500, 10,000, or 30,000 requests per second
   Currently, you cannot request a quota increate for KMS.
- Should be used for any files over 100MB and must be used for any file over 5GB
- Use MULTIPART UPLOADS to increase performance when uploading files to S3
- Use S3 BYTE-RANGE FETCHES to increase performance when downloading files to S3

S3 Select & Glacier Select

- Remember that S3 SELECT is used to retrieve only a subset of data from an object by using simple SQL expressions
- Save money on data transfer and increase speed

Some Best Practices With AWS Organizations

- Always enable multi-factor authentication on root account
- Always use a strong and complex password on root account
- Paying account should be used for billing purposes only. Do not deploy resources into the paying account
- Enable/Disable AWS services using Service Control Policies (SCP) either on Organization Units(HR, finance, etc) or on individual accounts

Sharing S3 Buckets Between Accounts

- Using Buckets Policies & IAM (applies across the entire bucket), Programmactic Access Only
- Using Bucket ACLs & IAM (individual objects), Programmatic Access Only
- Cross-account IAM Roles, Programmatic AND CONSOLE ACCESS. (create a role with a policy, assign it to a user, login to the user and switch)

Cross Region Replication(can also replicate in the same region)

- Versioning must be enabled on both the source and destination buckets
- Files in an existing bucket are not replicated automatically (old ones won’t be replicated)
- All subsequent updated files will be replicated automatically
- Delete markers are not replicated
- Deleting individual versions or delete markers will not be replicated
- Understand what Cross Region Replication is at a high level

S3 Lifecyle management

- Automates moving your objects between the different storage tiers.
- Can be used in conjunction with versioning
- Can be applied to current versions and previous versions
  S3 TRANSFER ACCELERATION: when you need to increase PERFORMANCE of your users being able to UPLOAD files to S3

CloudFront Overview

- Edge Location: This is the location where content will be cached. This is separate to an AWS Region/AZ
- Origin : This is the origin of all the files that the CDN will distribute. This can be either an S3 bucket, an EC2 Instance, an Elastic load Balancer, or Route53
- Distribution: This is the name given the CDN which consists of a collection of Edge Locations
  1- Web Distribution: Typically used for Websites
  2- RTMP: Used for media streaming
- Edge locations are not just READ only : you can write to them too (ie. put an object onto them)
- Objects are cached for the life of the TTL(Time To Live)
- You can clear cached objects, but you will be charged.

CloudFront Signed URLs and Cookies

- Use SIGNED URLs/COOKIES when you want to secure content so that only the AUTHORIZED PEOPLE are able to access it
- A signed URL is for individual files. 1 URL = 1 FILE
- A signed cookie is for multiple files. 1 COOKIE = MULTIPLE FILES
- if your origin is EC2, then use CloudFront

DATASYNC Overview

- Used to move large amount of data FROM ON-PREMISES TO AWS
- Used with NFS- and SMB-compatible file systems
- Replication can be done hourly, daily, or weekly
- Install the DataSync agent to start the replication
- Can be used to replicate EFS to EFS (EC2)

Snowball

- use when moving large amount of data into/out the AWS cloud
- snowvall can import S3, export from S3

Storage Gateway

- is a physical device, that connects legacy into S3
- File Gateway: for flat files, stored directly on S3
- Volumn Gateway
  - Stored Volumns: entire dataset is stored on site and is asynchronously backed up to S3
  - Cached Volumns: entire dataset is stored on S3 and the most frequently accessed data is cached on site
- Gateway Virtual Tape Library

Athena vs Macie

- Athena : used to running sql queries

  - is an interative query service
  - allows you to query data located in S3 using standard SQL
  - Serverless
  - commonly used to analyse log data stored in S3

- Macie : used as a security service to look for PII
  - uses AI to analyze data in S3 and helps identify PII(Personally Identifiable Information)
  - can also be used to analyse CloudTrail logs for suspicious API activity
  - includes Dashboards, reports and alerting
  - great for PCI-DSS compliance and preventing ID theft

READ THE S3 FAQ!!!

EC2

- Amazon Elastic Compute Cloud is a web service that provides resizable compute capacity in the cloud. Amazon EC2 reduces the time required to obtain and boot new server instances to minutes, allowing you to quickly scale capacity, both up and down, as your computing requirements change.

Pricing Types
1- On Damand: Allows you to pay a fixed rate by the hour(or by the second) with no commitment.
2- Reserved: Provides you with a capacity reservation, and offer a significant discount on the hourly charge for an instance. Contract Terms are 1 Year or 3 Year Terms
3- Spot: Enables you to bid whatever price you want for instance capacity, providing for even greater savings if your applications have flexible start and end times
4- Dedicated hosts: physical EC2 server dedicated for your use. Dedicated Hosts can help you reduce costs by allowing you to use your existing server-bound software licenses

- If the SPOT instance is terminated by Amazon EC2, you will not be charged for a partial hour of usage. However, if YOU TERMINATE the instance yourself, you will be charged for any hour in which the instance ran.

* Termination Protection is turned off by default, you must turn it on.
* On an EBS-backed instance, the default action is for the root EBS volume to be deleted when the instance is terminated
* EBS Root volumnes of your DEFAULT AMI's CAN be encrypted. You can also use a third party tool (such as bit locker etc) to encrypt the root volume, or this can be done when creating AMI's (lab to follow) in the AWS console or using the API.
* Additional volumes can be encrypted

Security Groups Basics

- All inbound traffic is blocked by default
- All outbound traffic is allowed
- Changes to Security Groups take effect immediately
- You can have any number of EC2 instances within in a security group
- You can have multiple security groups attached to EC2 Instances

- Security Groups are STATEFUL(when you open up a port both inbound and outbound open, this is not true for ACL)
- If you create an inbound rule allowing traffic in, that traffic is automatically allowed back out again
- You cannot block specific IP addresses using Security Groups, instead use Network Access Control Lists.
- You can sepcify ALLOW rules, but not deny rules.

EBS Types

- General Purpose SSD(gp2): most work loads
- Provisioned IOPS SSD(io1): highest performance, Databases
- Throughput Optimized HDD(st1): Big Data & Warehouses, Low cost HDD for frequent access
- Cold HDD(sc1): less frequent access, File servers, LOWEST COST
- EBS Macnetic(Standard): infrequent access, prev HDD version

Volumes & Snapshots

- Volumes exist on EBS. Think of EBS as a virtual hard disk
- Snapshots exist on S3. Think of snapshots as a photograph of the disk.
- Snapshots are point in time copies of Volumes
- Snapshots are INCREMENTAL : this means that only the blocks that have changed since your last snapshot are moved to S3
- If this is your first snapshot, It may take some time to create
- To create a snapshot for Amazon EBS volumes that serve as root devices(The one with OS), you should stop the instance before taking the snapshot
- However it is also possible take a snapshot while the instance is running
- You can create AMI's from both Volumes and Snapshots
- You can change EBS volume sizes on the fly, including the size and storage type
- Volumes will ALWAYS be in the same AZ as the EC2
- To move an EC2 volume from one AZ to another, take a snapshot of it, create an AMI from the snapshot and then use the AMI to launch the EC2 instance in a new AZ
- To move an EC2 volume from on region to another, take a snapshot of it, create an AMI from the snapshot and then copy the AMI from one region to the other. Then use the copied AMI to launch the new EC2 instance in the new region.

AMI Types(EBS vs Instance Store)

- Instance Store Volumes are sometimes called Ephemeral Storage(stopped, lose all data)
- Instance store volumes cannot be stopped. If the underlying host fails, you will lose your data.
- EBS based instances can be stopped. You will not lose the data on this instance if it is stopped
- You can reboot both, you will not lose data
- By default, both ROOT volumes will be delted on termination, However, with EBS volumes, you can tell AWS to keep the root device volume.

ENI(Elastic Network Interface) vs ENA(Elastic Network Adapter, Enhanced network) vs EFA(Elastic Fabric Adapter)

ENI : basic networking. perhaps you need a separate management network to your production network or a separate logging network and you need to do this at low cost. In this scenario use multiple ENIs for each network.
ENA : network speed 10Gbps ~ 100Gbps
EFA : HPC, machine learning , OS bypass

Encrypted Root Device Volumes & Snapshots

- Snapshots of encrypted volumes are encrypted automatically
- Volumes restored from encrypted snapshots are encrypted automatically
- You can share snapshots, but only if they are unencrypted
- These snapshots can be shared with other AWS accounts or made public
- You can encrypt root device volumes upon creation of the EC2 instance

* if not encrypted

- Create a snapshot of the unencrypted root device volume
- Create a copy of the snapshot and select the encrypt option
- Create an AMI from the encrypted Snapshot
- Use that AMI to launch new encrypted instances

Spot instances and Spot Fleets

- Spot instances save up to 90% of the cost of On-Demand Instances.
- Useful for any type of computing where you don't need persistent storage
- You can bllock Spot Instances from terminating by using Spot Block
- A Spot Fleet is a collection of Spot Instances and, optionally, On-Demand Instances.

EC2 Hibernate(taking the ram and saving to your EBS route volume, faster load up when rebooted)

- EC2 Hibernate preserves the in-memory RAM on persistent storage (EBS)
- Much faster to boot up because you do not need to reload the OS
- Instance families include C3,4,5,M3,4,5,R3,4,5
- Available for Windows, Amazon Linux 2 AMI, Ubuntu
- Instances can't be hibernated for more than 60 days
- Available for On-Demand instances and Reserved Instances

CloudWatch

- CloudWatch is used for monitoring performance
- CloudWatch can monitor most of AWS as well as your applications that run on AWS
- CloudWatch with EC2 will monitor events every 5 minutes by default
- You can have 1 minute intervals by turning on detailed monitoring
- You can create CloudWatch alarms which trigger notifications
- CloudWatch is all about PERFORMANCE. CloudTrail is all about AUDITING(CCTV)

- Standard Monitoring: 5 mins, Detailed Monitoring: 1 min
- Dashboards - creates dashboards to see what is happening with your AWS environment
- Alarms - allows you to set Alarms that notify you when particular thresholds are hit
- Events - CloudWatch Events helps you to respond to state changes in your AWS resources
- Logs - to aggregate, monitor, and store logs
- Cloudwatch monitors performance, CloudTrail monitors API calls in the AWS platform

AWS command Line

- You can interact with AWS from anywhere in the world just by using the command line
- You will need to set up access in IAM

Using IAM Roles with EC2

- Roles are more secure than storing your access key and secret access key on individual EC2 instances(CLI에서 configure 내용 볼수도 있으니깐)
- Roles are easier to manage
- Roles can be assigned to an EC2 instance after it is created using both the console & command line
- Roles are universal - you can use them in any region

EC2 Instance Meta Data

- Used to get information about an instance(such as public ip)
- curl http://169.254.169.254/latest/meta-data
- curl http://169.254.169.254/latest/user-data

Elastic File System

- Supports the Network File System version 4 (NFSv4) protocol
- You only pay for the storage you use (no pre-provisioning required)
- Can Scale up to the petabytes
- Can support thousands of concurrent NFS connections
- Data is stored across multiple AZ's within a region
- Read After Write Consistency

FSX for Windows & FSX for Lustre

- Windows FSx : When you need centralized storage for Windows-based applications such as Sharepoint, MS SQL Server, Workspaces, IIS Web Server or any other native Microsoft Application
- EFS: When you need distributed, hightly resilient storage for Linux instances and Linux-based applications
- Lustre FSx : When you nedd high-speed, high-capacity distributed storage. This will be for applications that do High Performance Compute(HPC), financial modelling etc. Remember that FSx for Lustre can store data directly on S3.

EC2 Placement Group

- Clustered Placement Group: Low Network latency / Hight Network Throughput (same AZ, region)
- Spread Placement Group: individual Critical EC2 instances
- Partitioned: multiple EC2 instances HDFS, HBase, and Cassandra

EC2 Placement Group2

- A clustered placement group can't span multiple AZ
- A spread placement and partitioned group can
- The name you specify for a placement group must be unique within your AWS account
- Only certain types of instances can be launched in a placement group (Compute Optimized, GPU, Memory Optimized, Storage Optimized)
- AWS recommend homogenous instances within clusred placement groups
- You can't merge placement groups
- You can move an existing instance into a placement group. Before you move the instance, the instance must be in the stopped state. You can move or remove an instance using the AWS CLI or an AWS SDK, you can't do it via the console yet.

HPC on AWS

- You can achieve HPC via DATA TRANSFER, COMPUTE AND NETWORKING, STORAGE, ORCHESTRATION AND AUTOMATION
- Data Transfer
  - Snowball, Snowmobile (tb/pb)
  - AWS Data Sync to store on S3, EFS, FSx for Windows etc
  - Direct Connect
- Compute & Networking
  - EC2 instances that are GPU or CPU optimized
  - EC2 fleets (Spot Instances or Spot Fleets)
  - Placement groups(cluster placement groups)
  - Enhanced networking single root I/O virtualization (SR-IOV)
  - Elastic Network Adaptors or Intel 82599 Virtual Function (VF) interface
  - Elastic Fabric Adapters
- Storage
  - Instance-attached stroage
    - EBS: Scale up to 64,000 IOPS with Provisioned IOPS(PIOPS)
    - Instance Store: Scale to millions of IOPS; low latency
  - Network storage
    - S3: Distributed object-based storage; not a file system
    - EFS: Scale IOPS based on total size, or use Provisionned IOPS
    - FSx for Lustre: HPC-optimized distributed file system; milicons of IOPs, which is also backed by S3
- Orchestration & Automation
  - AWS Batch
  - AWS Parallel Cluster

AWS WAF

- In the exam you will be given different scenarios and you will be asked how to block malicious IP addresses
  - Use AWS WAF : block IP, SQL attacks, Cross site scripting, individual countries
  - Use Network ACLs

RDS

RDB

- RDS runs on virtual machines
- You cannot log in to these OS however.
- Patching of the RDS OS and DB is Amazon's responsibility
- RDS is NOT Serverless
- Aurora Serveless IS SERVERLESS

RDS Backups, Multi-AZ & READ REPLICAS

- There are two different types of BACKUPS for rds
  - automated Backups
  - Database snapshots

READ REPLICAS

- used for scaling, not for DR!
- must have automatic backups turned on in order to deploy a read replica
- you can have up to 5 read replica copies of any database
- you can have read replicas of read replicas(but watch out for latency)
- each read replica will have its own DNS end point
- you can have read replicas that have Multi-AZ
- you can create rea replicas of Multi-AZ source databases
- Read replicas can be promoted to be their own databases. This breaks the replication
- you can have a read replica in a second region

- can be Multi-AZ
- used to increase performance
- must have backups turned on
- can be in different region
- can be MySQL, Postgre, MariaDB, Oracle, Aurora
- can be promoted to master, this will break the Read Replica

- Multi AZ
  - used for DR
  - you can force a failover from one AZ to another by rebooting the RDS instance

* Encryption at rest is supported for MySQL, Oracle, SQL Server, Postgre, MariaDB, Aurora. Encryption is done using the AWS Key Management Service (KMS) service. Once your RDS instance is encrypted, the data stored at rest in the underlying storage is encrypted, as are its automated backups, read replicas, and snapshots.

DynamoDB Basics

- Stored on SSD Storage
- Spread across 3 geographically distinct data centres
- Eventual Consistent Reads (default)
- Strongly Consistent Reads

DynamoDB Advanced (security)

- encryption at rest using KMS
- site-to-site VPN
- direct connect(DX)
- IAM policies and roles
- fine-grained access
- CloudWatch and CloudTrail
- VPC endpoints

Redshift

- business intelligence or dataWarehouse
- Available in only 1 AZ
  Redshift Backups
- enabled by default with a 1 day retention period
- max retention period is 35 days
- redshift always attempts to maintain at least three copies of your data (the original and replica on the compute nodes and a backup in Amazon S3)
- Redshift can also asynchronously replicate your snapshots to S3 in another region for disaster recovery.

Aurora

- 2 copies of data should be contained in each availability zone, with minimum of 3 availability zones, 6 copies of data
- you can share Aurora Snapshots with other AWS accounts
- 3 types of replicas available. Aurora replicas, MySQL replicas & PostgreSQL replicas, Automated failover is only available with Aurora Replicas
- Aurora has automated backups turned on by default. You can also take snapshots with Aurora, You can share these snapshots with other AWS accounts
- Use Aurora Serverless if you want a simple, cost-effective option for infrequent, intermittent, or unpredictable workloads

Elasticache

- use Elasticache to increase database and web application performance
- Redis is Multi-AZ
- You can do backups and restores of Redis

DMS(Database Migration Service)

- You do not need SCT if you are migrating to identical databases
- DMS allows you to migrate databases from one source to AWS
- The source can either be on-premises, or inside AWS itself or another cloud provider such as Azure
- You can do homogenous migrations or heterogeneous migrations
- If you do a heterogeneous migration, you will need the AWS Schema Conversion Tool(SCT)

Caching(uptodate, accurate, latency) Strategies on AWS

- services that have caching capabilities: CloudFront, API Gateway, ElastiCache(Memcached and Redis), DynamoDB Accelerator(DAX)

EMR(Elastic Map Reduce) Overview

- EMR is used for big data processing
- Consists of a master node, a core node, and (optionally) a task node
- By default, log data is stored on the master node
- you can configure replication to S3 on five-minute intervals for all log data from the master node; however, this can only be configured when creating the cluster for the first time

Advanced IAM

AWS Directory Service

- Active directory
- connect AWS resources with on-premises AD
- SSO to any domain-joined EC2 instance
- AWS Managed Microsoft AD
- AD trust
- AWS vs suctomer responsibility
- Simple AD does not support trusts
- Cloud Directory
- Cognito user pools
- AD vs Non-AD compatible services
  - AD Compatible
    - Managed Microsoft AD(a.k.a., Directory Service for Microsoft Active Directory)
    - Ad Connector
    - Simple AD
  - Not AD Compatible
    - Cloud Directory
    - Cognito user pools

IAM Policies

- ARN
- IAM policy structure
- effect/action/resource
- identity vs resource policies
- any permissions are not explicitly allowd (= implicitly denied)
- explicit deny > everything else
- only attached policies have effect
- aws joins all applicable policies
- AWS-managed vs customer-managed policies
- Permission Boundaries
  - used to delegate administration to other users
  - prevent privilege escalation or unnecessarily broad permissions
  - control MAXIMUM permissions an IAM policy can grant
  - use cases
    - developers creating roles for Lambda functions
    - Application owners creating roles for EC2 instances
    - admins creating ad hoc users

Resource Access Manager(RAM)

- Resource sharing between accounts
- Individual accounts and AWS organizations
- Types of resources you can share

AWS Single Sign-On

- centrally namage access
- ex. G Suite, Office 365, etc
- use existing identities
- account-level permissions
- If you see SAML2.0, the answer is SSO

DNS

- ELBs do not have pre-defined IPv4 addresses; you resolve to them using a DNS name.
- Understand the difference between an Alias Record and a CNAME
- Given the choice, always choose an Alias Record over a CNAME
- Common DNS types
  - SOA Records
  - NS Records
  - A Records
  - CNAMES
  - MX Records
  - PTR Records
* Alias Record:
* CNAME: 
Route53 - Register a Domain Name

- You can buy domain names directly with AWS
- It can take up to 3 days to register er depending on the circumstances

Routing Policies

- Simple Routing
  - If you choose the simple routing policy you can only have ONE RECORD WITH MULTIPLE IP ADDRESSES. If you specify multiple values in a record, Routh53 return all values to the user in a random order
- Weighted Routing
  - Allows you split your traffic based on different weights assigned(you set 10% of your traffic to go to US-EAST-1 and 90% to go to EU-WEST-1)
  - Health Checks
    - You can set health checks on individual record sets
    - If a record set fails a health check it will be removed from Route53 until it passes the health check
    - You can set SNS NOTIFICATION to alert you if a health check is failed
- Latency-based Routing
  - Route53 sends the traffic to EU-WEST-2 because it has A MUCH LOWER LATENCY than AP-SOUTHEAST-2
- Failover Routing
  - ACTICE-PASSIVE strategy
- Geolocation Routing
  - lets YOU CHOOSE WHERE your traffic will be sent BASED ON THE GEO LOCATION of your users.
- Geoproximity Routing (Traffic Flow Only)
  - lets Route53 traffic to your resources based on the geo location of your users and resources.
  - optionally CHOOSE to route more traffic or less to a given resource by setting 'BIAS'
  * to use this routing, you must use 'Route53 TRAFFIC FLOW'
- Multivalue Answer Routing
  - lets you configure Route53 to return multiple values, such as IP addresses for your web, in response to DNS queries. You can specify multiple values for almost any record, but multivalue answer routing also lets you check the health of each resource, so Route 53returns only values for healthy resources
  * SIMILAR to SIMPLE ROUTING but allows to put health checks on each record set

VPC

Introduction to VPCs

- Think of a VPC as a logical datacenter in AWS
- Consists of IGWs(Or Virtual Private Gateways), Route Tables, Network Access Control Lists, Subnets, and Security Groups
- 1 Subnet = 1 Availability Zone
- Security Groups are STATEFUL; Network Access Control Lists are STATELESS.
- NO TRANSITIVE PEERING (you cannot go through)

Build A Custom VPC(1)

- When you create a VPC a default Route Table, Network Access Control List(NACL) and a default Security Group are created
- It creates NO SUBNETS, nor will it create NO DEFAULT INTERNET GATEWAY
- US-East-1A in your AWS account can be a COMPLETELY DIFFERENT availability zone to US-East-1A in another AWS account. The AZ's are randomized
- Amazon always reserve FIVE IP ADDRESSES WITH IN YOUR SUBNETS
- You can only have ONE INTERNET GATEWAY per VPC
- Security Groups can't span VPCs

Build A Custom VPC(2)

- NAT Instances
  - When creating a NAT instance, Disable Source/Destination Check on the Instance
  - NAT instances must be in a public subnet
  - There must be a route out of the private subnet to the NAT instance.
  - The amount of traffic that NAT instances can support DEPENDS ON THE INSTANCE SIZE. If you are bottlenecking, increase the instance size.
  - You can create high availability using Autoscaling Groups, multiple subnets in different AZs, and a script to automate failover
  - Behind a Security Group
- NAT Gateways
  - Redundant inside the Availability Zone
  - Preferred by the enterprise
  - Starts at 5Gbps and scales currently to 45Gbps
  - No need to patch
  - NOT ASSOCIATED WITH SECURITY GROUPS
  - AUTOMATICALLY assigned A PUBLIC IP address
  - Remember to update your route tables
  - No need to disable Source/Destination Checks
  - create a NAT gateway IN EACH AZ for High Availability

Access Control Lists

- Your VPC automatically comes with a default network ACL, and by default it ALLOWS ALL INBOUND AND OUTBOUND traffic
- You can create custom network ACLs. By default, each custom network ACL denies all inbound and outbound traffic until you add rules
- Each subnet must be associated with network ACL. If you don't explicitly associate a subnet with a network ACL, the subnet is automatically associated with the default network ACL.
- Block IP Addresses using network ACLs, not Security Groups
- You can associate a network ACL with multiple subnets; however a subnet can be associated with ONLY ONE network ACL at a time.
- Network ACLs contain a numbered list of rules that is EVALUDATED IN ORDER, starting with the lowest numbered rule
- Network ACLs have separate inbound and outbound rules, and each rule can either allow or deny traffic
- Network ACLs are stateless; responses to allowed inbound traffic are subject to the rules for outbound traffic(and vice versa)

* ELB must be assigned to AT LEAST TWO subnets

VPC Flow Logs

- You cannot enable flow logs for VPCs that are peered with your VPC unless the peer VPC is in your account
- You can tag flow logs
- After you've created a flow log, you CANNOT CHANGE ITS CONFIGURATION; for example, you can't associate a different IAM role with the flow log
- Not all IP Traffic is monitored
  - Traffic generated by instances when they contact the Amazon DNS server. If you use your own DNS server, then all traffic to that DNS server is logged
  - Traffic generated by a Windows instance for Amazon Windows license activation
  - Traffic to and from 169.254.169.254 for instance metadata
  - DHCP traffic
  - Traffic to the reserved IP address for the default VPC router

NAT vs Bastions

- A NAT Gateway or NAT Instance is used to provide internet traffic to EC2 instances in a private subnets
- A Bastion is used to securely administer EC2 instances (Using SSH or RDP). Bastions are called Jump Boxes in Australia
- You cannot use a NAT Gateway as a Bastion host

* 특정 IP만 접근 가능하도록 설정이 가능하다.

  Direct Connect

- Direct Connect directly connects your data center to AWS
- Useful for HIGH THROUGHPUT WORKLOADS(ie lots of network traffic)
- Or if you need a STABLE and RELIABLE SECURE connection

Setting Up A VPN Over a Direct Connect Connection

- Steps to creating a direct connect connection

  - Create a vitual interface in the direct connect console. This is a PUBLIC VIRTUAL INTERFACE
  - Go to the VPC console and then to VPN connections. Create a CUSTOMER GATEWAY
  - Create a VIRTUAL PRIVATE GATEWAY
  - Attach the VIRTUAL PRIVATE GATEWAY then to the desired VPC
  - Select VPN Connections and create new VPN CONNECTION
  - Select the VIRTUAL PRIVATE GATEWAY and the CUSTOMER GATEWAY
  - Once the VPN is available, setup the VPN on the CUSTOMER GATEWAY or FIREWALL

  Global Accelerator

  - AWS Global Accelerator is a service in which you create accelerators to improve availability and performance of your applications for local and global users.
  - You are assigned TWO STATIC IP ADDRESSES (or alternatively you can bring your own)
  - You can control traffic using TRAFFIC DIALS. This is done within the endpoint group
  - Global Accelerator improves performance for a wide range of applications over TCP or UDP by proxying packets at the edge to applications running in one or more AWS Regions. Global Accelerator is a good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP, as well as for HTTP use cases that specifically require static IP addresses or deterministic, fast regional failover.

  VPC End Points(Interface Endpoints, Gateway Endpoints)

  - An interface endpoint is an elastic network interface with A PRIVATE IP ADDRESS that serves as an ENTRY POINT for traffic destined to a supported service.
  - GATEWAY ENDPOINTS support S3 and DynamoDB
  - A VPC endpoint enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by PRIVATELINK without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect. Instances in your VPC do not require a public IP to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.

  - Endpoints are virtual devices. They are horizontally scaled, redundant, and hightly available VPC components that allow communication between instances in your VPC and services without imposing availability risks or bandwidth constraints on your network traffic

VPC Private Link

- If asking about peering VPCs to tens, hundreds, of customer VPCs, think of AWS PrivateLink
- Doesn't require VPC peering; no route tables, NAT, IGWs, etc
- Requires a Network Load Balancer on the service VPC and an ENI(Elastic Network Interface) on the customer VPC

Transit Gateway

- Allows you to have transitive peering between thousands of VPCs and on-premises data centers
- works on a hub-and-spoke model
- works on a regional basis, but you can have it across multiple regions
- you can use it across multiple AWS accounts using RAM(Resource Access Manager)
- you can use ROUTE TABLES to limit how VPCs talk to one another
- works with direct connect as well as VPN connections
- support IP multicast (not supported by any other AWS service)

* P multicast is a method of sending Internet Protocol (IP) datagrams to a group of interested receivers in a single transmission.

VPN Hub

- If you have multiple sites, each with its own VPN connection, you can use AWS VPN CloudHub to connect those sites together(Hub-and-spoke model)
- operates over the public internet, but all traffic between the customer gateway and the AWS VPN CloudHub is encrypted

Networking Costs on AWS

- Use private IP over public IP to save on costs. This then utilizes the AWS backbone network
- If you want to cut all network costs, group your EC2 instances in the Same AZ and use private IP addresses. This is cost-free, but make usre to keep in mind single point of failure issues

HA Architecture

3 Different Types Of Load Balancers

- ALB(Application Load Balancers) : HTTP/HTTPS, Layer7, Intelligent
- NLB(Network Load Balancers) : TCP, Layer 4(connection level), low-latencies, used for extreme performance
- Classic Load Balancers : legacy, if app stops responding, 504 error
  - 504 means the gateway has timed out. This means that the application not responding within the idle timeout period(web server or db server)
  - If you need the IPv4 address of your end user, look for the X-Forwarded-For header
  - Instances monitored by ELB are reported as: InService / OutofService
  - Health Checks check the instance health by talking to it
  - Load Balances have their own DNS name. You are never given an IP address
  - Read the ELB FAQ for Classic Load Balancers

Advanced Load Balancer Theory

- Sticky Sessions enable your users to stick to the same EC2 instance. can be useful if you are storing information locally to that instance
- Cross Zone Load Balancing enables you to load balance across multiple availability zones
- Path patterns allow you to direct traffic to different EC2 instances based on the URL contained in the request.

CloudFormation

- is a way of completely scripting your cloud environment
- Quick Start is a bunch of CloudFormation templates already built by AWS Solutions Architects allowing you to create complex environments very qucikly

Elatic Beanstalk

- you can quickly deploy and manage applications in the AWS Cloud without worrying about the infrastucture that runs those applications. You simply upload your application, and Elastic Beanstalk automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring.

HA Bastion

- Two hosts in two separate Availabiligy Zones. Use a Network Load Balancer with static IP addresses and health checks to fail over from one host to the other.
- Can't use an Application Load Balancer, as it is layer 7 and you need to use layer 4
- One host in one Availability Zone behind an Auto Scaling group with health checks and a fixed EIP. If the host fails, the health check with fail and the Auto Scaling group will provision a new EC2 instance in a separate Availability Zone. You can use a user data script to provision the same EIP to the new host. This is the cheapest option, but it is not 100% fault tolerant.

You need to be aware of what high-level AWS services you can use on-premises for the exam:

- Database Migration service
- Server migration Service
- AWS Application Discovery Service
- VM import/export
- download Amazon Linux 2 as an ISO

Applications

SQS

- SQS is as way to de-couple your infrastructure
- SQS is pull based, not pushed based
- Messages are 256 KB in size
- Messages can be kept in the queue from 1 minute to 14 ddays; the default retention period is 4 days
- Standard SQS and FIFO SQS
- Standard order is not guaranteed and messages can be delivered more than once
- FIFO order is strictly maintained and messages are delivered only once
- Visibility Time Out is the amount of time that the message is invisible in the SQS queue after a reader picks up that message. Provided the job is processed before the visibility time out expires, the message will then be deleted from the queue. If the job is not processed within that time, the message will become visible again and another reader will process it. This could result in the same message being delivered twice.
- Visibility timeout maximum is 12 hours
- SQS guarantees that your messages will be processed at least once.
- Amazon SQS long polling is a way to retrieve messages from your Amazon SQS queues. While the regular short polling returns immediately (even if the message queue being polled is empty), long polling doesn't return a response until a message arrives in the message queue, or the long poll times out.

SWF vs SQS

- SQS has a retention period of up to 14 days; with SWF, workflow excutions can last up to 1 year
- Amazon SWF presents a TASK-ORIENTED API, whereas Amazon SQS offers a MESSAGE-ORIENTED API
- Amazon SWF ensures that a task is assigned only once and is never duplicated. With Amazon SQS, you need to handle duplicated messages and may also need to ensure that a message is processed only once.
- Amazon SWF keeps track of all the tasks and events in an application. With Amazon SQS, you need to implement your own application-level tracking, especially if your application uses multiple queues.

SWF Actors

- Workflow Starters: Application that can initiate(start) a workflow. Could be your e-commerce website following the placement of an order, or a mobile app searching for bus times.
- Deciders : Control the flow activity tasks in a workflow execution. If something has finished (or failed) in a workflow, a Decider decides what to do next.
- Activity Workers : carry out the activity tasks

SNS Benefits

- Instantaneous, push-based delivery(no polling)
- Simple APIs and easy integration with applications
- Flexible message delivery over multiple transport protocols
- Inexpensive, pay-as-you-go model with no up-front costs
- Web-based AWS management console offers the simplicity of a point-and-click interface.

SNS vs SQS

- Both Messaging Services in AWS
- SNS : PUSH
- SQS : Polls(PULL)

Elastic Transcoder

- a media transcoder in the cloud. it converts media files from their original source format into different formats that will play on smartphones, tablets, PCs, etc.

API Gateway

- high level
- has caching capabilities to increase performance
- is low cost and scales automatically
- you can throttle API Gateway to prevent attacks
- you can log results to CloudWatch
- If you are using JS/AJAX that uses multiple domains with API Gateway, ensure that you have enabled CORS on API Gateway
- CORS is enforced by the client

Kinesis

- Know the difference between Kinesis Streams and Kinesis Firehose
  
  - Kinesis Streams: store and analyze streamed data in Shards(EC2 instances) for 24 hours by default.(can store upto 7 days) After, they can be sent to DynamoDB, S3, EMR or RedShift.

  - Kinesis Firehose

  Your organization needs to ingest a big data stream into their data lake on Amazon S3. The data may stream in at a rate of hundreds of megabytes per second. What AWS service will accomplish the goal with the least amount of management?
  Amazon Kinesis Firehose*
  Amazon Kinesis Streams
  Amazon CloudFront
  Amazon SQS
  Your organization is looking for a solution that can help the business with streaming data several services will require access to read and process the same stream concurrently. What AWS service meets the business requirements?
  Amazon Kinesis Firehose
  Amazon Kinesis Streams*
  Amazon CloudFront
  Amazon SQS
  Your application generates a 1KB JSON payload that needs to be queued and delivered to EC2 instances for applications. At the end of the day, the application needs to replay the data for the past 24 hours. In the near future, you also need the ability for other multiple EC2 applications to consume the same stream concurrently. What is the best solution for this?
  Kinesis Data Streams\*
  Kinesis Firehose
  SNS
  SQS

- Kinesis Analytics is Amazon’s forthcoming product offering that will allow running of standard SQL queries against data streams, and send that data to analytics tools for monitoring and alerting. This product has not yet been released, and Amazon has not published details of the service as of this date.

Cognito

- Federation allows users to authenticate with a Web Identity Provider(Google, Facebook, Amazon)
- The user authenticates first with the Web ID Provider and receives an authentication token, which is exchanged for temporary AWS credentials allowing them to assume an IAM role.
- Cognito is an Identity Broker which handles interaction between your applications and the Web ID provider (You don't need to write your own code to do this)
- User pool is user based. It handles things like user registration, authentication, and account recovery
- Identity pools authorise access to your WS resources

- Lambda
- Lambda scales out (not up) automatically
- Lambda functions are independent, 1 event = 1 function
- Lambda is serverless
- Know what services are serveless
- Lambda functions can trigger other lambda functions, 1 event can = x functions if fucntions trigger other functions
- Architecture can get extremely complicated, AWS X-ray allows you to debug what is happening
- Lambda can do things globally, you can use it to back up S3 buckets to other S3 buckets etc
- Know your triggers

------------------------------------------------------------

- create a DECOUPLED architecture for applications which make use of both AWS and on-prem resources
-- use SQS and SWF

- encrypt all the DB credentials, API keys and other secrets and rotate them on a regular basis to improve data security
-- use aws Secrets Manager to store and encrypt them. Enable automatic rotation for all of the credentials

- when web service clients can only access trusted IP addresses whitelisted on their firewalls.
-- Asscociate an EIP to a Network Load Balancer

- big data processing frameworks to analyze vast amounts of semi-structured, structured data and access it using BI tools and SQL
-- Create an Amazon EMR cluster and store the processed data in Amazon RedShift

- Perfect Forward Secrecy is used to offer SSL/TLS cipher suites for which AWS services?
-- CloudFront and Elastic Load Balancing

- users around the world, secure your application by allowing multiple domains to serve SSL traffic over the same IP address
-- Generate an SSL certificate with AWS Certificate Manager and create a CloudFront web distribution. Associate the certificate with your web distribution and enable the support for Server Name Indication(SNI)

- An organization needs to control the access for several S3 buckets. They plan to use a gateway endpoint to allow access to trusted buckets.
-- Generate an endpoint policy for trusted S3 buckets

- minimize the impact of DDoS attacks
-- To detect and mitigate DDoS attacks, you can use AWS WAF in addition to AWS Shield.

Q: 언제 AWS DataSync를 사용하고 언제 AWS Storage Gateway를 사용해야 합니까?

A: AWS DataSync를 사용하여 기존 데이터를 Amazon S3로 마이그레이션한 다음, 이후 AWS Storage Gateway의 파일 게이트웨이 구성을 사용하여 마이그레이션된 데이터 및 온프레미스 파일 기반 애플리케이션의 지속적인 업데이트에 대한 액세스를 유지합니다.

Q: 언제 AWS DataSync를 사용하고 언제 Amazon S3 Transfer Acceleration을 사용해야 합니까?

A: 애플리케이션이 이미 Amazon S3 API에 통합되어 있고, S3로의 대용량 파일 전송을 위해 더 높은 처리량을 원할 경우 S3 Transfer Acceleration을 사용할 수 있습니다. 기존 스토리지 시스템(예: Network Attached Storage) 또는 변경될 수 없는 기기(예: DNA 시퀀서, 비디오 카메라)로부터 데이터를 전송하고자 하는 경우 또는 여러 개의 대상을 원하는 경우 AWS DataSync를 사용할 수 있습니다. 또한 DataSync는 데이터 전송을 자동화 및 간소화합니다. 이를 위해 기본 제공 재시도 및 네트워크 복원력 메커니즘, 데이터 무결성 검증, 특정 요구를 충족하는 유연한 구성(예: 대역폭 조절 등)과 같은 추가적인 기능을 제공합니다.

- system is hosted in AWS and uses ECS to host its front-end tier and an RDS configured with Multi-AZ for its database tier. What are the events that will make Amazon RDS automatically perform a failover to the standby replica? (Select TWO.)
-- Storage failure on primary
-- Loss of availability in primary Availability Zone

- issue short-lived access tokens that act as temporary security credentials to allow access to your AWS resources?
-- Use AWS STS
* AWS Security Token Service (AWS STS) is the service that you can use to create and provide trusted users with temporary security credentials that can control access to your AWS resources.

***You have three mutually exclusive options depending on how you choose to manage the encryption keys:
Use Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3)
Use Server-Side Encryption with AWS KMS-Managed Keys (SSE-KMS)
Use Server-Side Encryption with Customer-Provided Keys (SSE-C)

- establish a site-to-site VPN connection. What needs to be configured outside of the VPC for them to have a successful site-to-site VPN connection?

-- A customer gateway is a physical device or software application on your side of the VPN connection. To create a VPN connection, you must create a customer gateway resource in AWS, which provides information to AWS about your customer gateway device. Next, you have to set up an Internet-routable IP address (static) of the customer gateway's external interface.

- A company needs to assess and audit all the configurations in their AWS account. It must enforce strict compliance by tracking all configuration changes made to any of its Amazon S3 buckets. Publicly accessible S3 buckets should also be identified automatically to avoid data breaches.
-- Use AWS Config to set up a rule in your AWS account.

- best way to allow the EC2 instance to access the S3 bucket and other AWS services
-- Create a role in IAM and assign it to the EC2 instance

- Auto Scaling group of Amazon EC2 instances. Several instances are failing due to insufficient swap space. you are instructed to troubleshoot the issue and effectively monitor the available swap space of each EC2 instance.
-- install the CloudWatch agent on each instance and monitor the SwapUtilization metric.

* In Amazon SQS, you can configure the message retention period to a value from 1 minute to 14 days. The default is 4 days. Once the message retention limit is reached, your messages are automatically deleted.
A single Amazon SQS message queue can contain an unlimited number of messages. However, there is a 120,000 limit for the number of inflight messages for a standard queue and 20,000 for a FIFO queue. Messages are inflight after they have been received from the queue by a consuming component, but have not yet been deleted from the queue.
In this scenario, it is stated that the SQS queue is configured with the maximum message retention period. The maximum message retention in SQS is 14 days that is why the option that says: Tell the users that the application will be operational shortly and all received requests will be processed after the web application is restarted is the correct answer i.e. there will be no missing messages.

- monitor a few database metrics and then afterward, send email notifications
-- Amazon CloudWatch and Amazon Simple Notification Service. NOT SES!

* KNOW THE DIFFERENCES AOMONG 

* Combined with the monitoring features of Amazon CloudWatch Events and AWS CloudTrail, Amazon DLM provides a complete backup solution for EBS volumes at no additional cost.

- A Solutions Architect is working for a company that uses Chef Configuration management in their data center. She needs to leverage their existing Chef recipes in AWS.
-- AWS OpsWorks == Chef and Puppet. ; JUST MEMORIZE THIS!

* Take note as well that the four EC2 instances all belong to a public non-default subnet. Which means that a new EC2 instance will not have a public IP address by default since the since IPv4 public addressing attribute is initially set to false.

- the prerequisites for routing traffic to a website that is hosted in an Amazon S3 Bucket:

-- An S3 bucket that is configured to host a static website. The bucket must have the same name as your domain or subdomain. For example, if you want to use the subdomain portal.tutorialsdojo.com, the name of the bucket must be portal.tutorialsdojo.com.

-- A registered domain name. You can use Route 53 as your domain registrar, or you can use a different registrar.

-- Route 53 as the DNS service for the domain. If you register your domain name by using Route 53, we automatically configure Route 53 as the DNS service for the domain.

- wide range of products and services, use an API gateway. What are the features of API gateway related to this situation?
-- you pay only for the API calls you receive and the amount of data transferred out
-- enables you to build RESTful APIs and WebSocket APIs that are optimized for serverless workloads


* EBS volumes can be attached to any EC2 Instance in any Availability Zone is incorrect as EBS volumes can only be attached to an EC2 instance in the same Availability Zone.

 - An accounting application uses an RDS database configured with Multi-AZ deployments to improve availability. What would happen to RDS if the primary database instance fails?
 -- The canonical name record(CNAME) is switched from the primary to standby instance.

- tried to deploy a new Amazon EC2 instance but she received an error saying that there is no IP address available on the subnet.
 -- By default, a new EC2 instance uses an IPv4 addressing protocol. To fix the problem in the scenario, you need to create a new IPv4 subnet and deploy the EC2 instance in the new subnet.