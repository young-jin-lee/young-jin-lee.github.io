---
layout: post
title: "NGROK"
comments: true
categories: Other
---


IAM
- IAM is universal. It does not apply to regions at this time.
- The "root account" is simply the account created when first setip your AWS account. It has complete Admin access
- New Users have NO permissions when first created
- New Users are assigned Access Key ID & Secret Access Keys when first created
- There are not the same as a password. You cannot use the Access key ID & Secret Access Key to Log into the console. You can use this to access AWS via the APIs and Comman Line, however. 
- You only get to view these once. If you lose them, you have to regenerate them. So, save them in a secure location

- Always setup Multifactor Authentication on your root account
- You can create and customise your own password rotation policies.

Aws
Automatic notifications : Cloud Watch → Billing Alarm(using SNS topic)

S3
-	S3 is Object-based: i.e. allows you to upload files
-	Files can be from 0 bytes to 5tb
-	There is unlimited storage
-	Files are stored in Buckets
-	S3 is a universal namespace. That is, names must be unique globally
-	Not suitable to install an OS on
-	Successful uploads will generate a HTTP 200 status code.
-	You can turn on MFA Delete
-	Read after Write consistency for PUTS of new objects
-	Eventual Consistency for overwrite PUTS and DELETES(can take some time to propagate)
-	Bucket names share a common name space. You cannot have the same bucket name as someone else
-	When you view your buckets you view them globally but you can have buckets in individual regions
-	You can replicate the contents of one bucket to another bucket automatically by using cross region replication
-	You can change storage classes and encryption of your objects on the fly

Restricting bucket Access
-	Bucket Policies – applies across the whole bucket
-	Object Policies – applies to individual files
-	IAM Policies to Users & Groups – applies to Users & Groups

S3 classes
-	S3 Standard: 99.99 availability, 11 99 durability
-	S3-IA : Infrequently Acccessed, requires rapid access when needed, lower fee than S3, charged a retrieval fee
-	S3 One Zone – IA : RRS
-	S3 – Interlligent Tiering
-	S3 Glacier : retrieval time from minutes to hours
-	S3 Glacier Deep Archive : cheapest, retrieval time 12 hours

* Amazon Glacier: an aws service designed for long term data archival

Key Fundamentals of S3
-	Key, value (simply the data and is made up of a sequence of bytes)
-	Version ID 
-	Metadata
-	Subresources; Access Control List, torrent

S3 Security And Encryption
-	Encryption In Transit is Achieved by : SSL/TLS
-	Encryption At Rest (Server Side) is Achieved by: 
	S3 Managed keys – SSE-S3
	AWS Key Management Service, Managed Keys – SSE-KMS
	Server Side Encryption With Customer Provided Keys – SSE-C
-	Client Side Encryption

S3 Version Control
-	Stores all versions of an object (including all writes and even if you delete an object)
-	Great backup tool
-	Once enables, versioning cannot be disabled, only suspended
-	Integrates with Life cycle rules
-	Versioning’s MFA Delete capability, which uses multi-factor authentication, can be used to provide an additional layer of security



S3 Lock Policies & Glacier Vault Lock
-	Use S3 Object Lock to store objects using a write once, read many (WORM) model (prevent objects from being deleted or modified for a fixed amount of time or indefinitely.)
-	Object locks can be on individual objects or applied across the bucket as a whole
-	Objects locks come in two modes: governance mode and compliance mode.
-	With governance mode, users can’t overwrite or delete an object version or alter its lock settings unless they have special permissions.
-	With compliance mode, a protected object version can’t be overwritten or deleted by any user, including the root user in your AWS account during retention periods.
-	Retention Period protects an object version for a fixed amount of time
-	A legal hold prevents an object version from being overwritten or deleted
-	S3 Glacier Vault Lock allows you to easily deploy and enforce compliance controls for individual S3 Glacier vaults with a Vault Lock policy. You can specify controls such as WORM in a Vault Lock policy and lock the policy from future edits. Once locked, the policy can no longer be changed. 


S3 Performance
-	Prefixes : mybucketname/folder1/subfolder1/myfile.jpg → /folder1/subfolder1
-	The more prefixes you use, the better performance you get out of S3
-	You can also achieve a high number of requests : 3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second per prefix
-	You can get better performance by spreading your reads across different prefixes. For example, if you are using two prefixes, you can achieve 11,000 requests per second.
-	S3 limitations when using SSE-KMS to encrypt you objects in S3
	Uploading/downloading will count toward the KMS quota
	Region-specific, however, it’s either 5,500, 10,000, or 30,000 requests per second
	Currently, you cannot request a quota increate for KMS.
-	Use MULTIPART UPLOADS to increase performance when uploading files to S3
-	Should be used for any files over 100MB and must be used for any file over 5GB
-	Use S3 BYTE-RANGE FETCHES to increase performance when downloading files to S3

S3 Select & Glacier Select 
-	Remember that S3 Select is used to retrieve only a subset of data from an object by using simple SQL expressions
-	Get data by rows or columns using simple SQL expressions
-	Save money on data transfer and increase speed

Some Best Practices With AWS Organizations
-	Always enable multi-factor authentication on root account
-	Always use a strong and complex password on root account
-	Paying account should be used for billing purposes only. Do not deploy resources into the paying account
-	Enable/Disable AWS services using Service Control Policies (SCP) either on Organization Units(HR, finance, etc) or on individual accounts

Sharing S3 Buckets Between Accounts
-	Using Buckets Policies & IAM (applies across the entire bucket), Programmactic Access Only
-	Using Bucket ACLs & IAM (individual objects), Programmatic Access Only
-	Cross-account IAM Roles, Programmatic AND Console access. (create a role with a policy, assign it to a user, login to the user and switch)

Cross Region Replication(can also replicate in the same region)
-	Versioning must be enabled on both the source and destination buckets
-	Files in an existing bucket are not replicated automatically (old ones won’t be replicated)
-	All subsequent updated files will be replicated automatically
-	Delete markers are not replicated
-	Deleting individual versions or delete markers will not be replicated
-	Understand what Cross Region Replication is at a high level

S3 Lifecyle management
-	Automates moving your objects between the different storage tiers.
-	Can be used in conjunction with versioning
-	Can be applied to current versions and previous versions

S3 TRANSFER ACCELERATION: when you need to increase performance of your users being able to upload files to S3


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
- Use SIGNED URLs/COOKIES when you want to secure content so that only the people you authorize are able to access it
- A signed URL is for individual files. 1 file = 1 URL
- A signed cookie is for multiple files. 1 cookie = multiple files
- if your origin is EC2, then use CloudFront

DATASYNC Overview
- Used to move large amount of data from on-premises to AWS
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

REAT THE S3 FAQ!!!


EC2

- Amazon Elastic Compute Cloud is a web service that provides resizable compute capacity in the cloud. Amazon EC2 reduces the time required to obtain and boot new server instances to minutes, allowing you to quickly scale capacity, both up and down, as your computing requirements change. 

Pricing Types
1- On Damand: Allows you to pay a fixed rate by the hour(or by the second) with no commitment.
2- Reserved: Provides you with a capacity reservation, and offer a significant discount on the hourly charge for an instance. Contract Terms are 1 Year or 3 Year Terms
3- Spot: Enables you to bid whatever price you want for instance capacity, providing for even greater savings if your applications have flexible start and end times 
4- Dedicated hosts: physical EC2 server dedicated for your use. Dedicated Hosts can help you reduce costs by allowing you to use your existing server-bound software licenses
* If the SPOT instance is terminated by Amazon EC2, you will not be charged for a partial hour of usage. However, if YOU TERMINATE the instance yourself, you will be charged for any hour in which the instance ran.

- Termination Protection is turned off by default, you must turn it on.
- On an EBS-backed instance, the default action is for the root EBS volume to be deleted when the instance is terminated
- EBS Root volumns of your DEFAULT AMI's CAN be encrypted. You can also use a third party tool (such as bit locker etc) to encrypt the root volume, or this can be done when creating AMI's (lab to follow) in the AWS console or using the API.
- Additional volumes can be encrypted

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
- Cold HDD(sc1): Lowest HDD, less frequent access, File servers, LOWEST COST
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
- Snapshots of encrpted volumes are encrypted automatically
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

CloadWatch
- CloudWatch is used for monitoring performance
- CloudWatch can monitor most of AWS as well as your applications that run on AWS
- CloudWatch with EC2 will monitor events every 5 minutes by default
- You can have 1 minute intervals by turning on detailed monitoring
- You can create CloudWatch alarms which trigger notifications
- CloudWatch is all about performance. CloudTrail is all about auditing(CCTV)

- Standard Monitoring: 5mins, Detailed Monitoring: 1min
- Dashboards - creates dashboards to see what is happening with your AWS environment
- Alarms - allows you to set Alarms that notify you when particular thresholds are hit
- Events - CloudWatch Events helps you to respond to state changes in your AWS resources
- Logs - to aggregate, monitor, and store logs
- Cloud watch monitors performance, CloudTrail monitors API calls in the AWS platform

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
- curl http://169.254.169.2 54/latest/user-data

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
- must have backups turned o
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

EMR(Elastic Map Reduce ) Overview
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

Route53 - Register a Domain Name
- You can buy domain names directly with AWS
- It can take up to 3 days to register er depending on the circumstances

Routing Policies
- Simple Routing
    - If you choose the simple routing policy you can only have one record with multiple IP addresses. If you specify multiple values in a record, Routh53 return all values to the user in a random order
- Weighted Routing
    - Allows you split your traffic based on different weights assigned(you set 10% of your traffic to go to US-EAST-1 and 90% to go to EU-WEST-1)
    - Health Checks
        - You can set health checks on individual record sets
        - If a record set fails a health check it will be removed from Route53 until it passes the health check
        - You can set SNS notifications to alert you if a health check is failed
- Latency-based Routing
    - Route53 sends the traffic to EU-WEST-2 because it has a much lower latency than AP-SOUTHEAST-2
- Failover Routing
    - Active-passive strategy
- Geolocation Routing
    - lets you choose where your traffic will be sent based on the geo location of your users.
- Geoproximity Routing (Traffic Flow Only)
    - lets Route53 traffice to your resources based on the geo location of your users and resources.
    - optionally choose to route more traffic or less to a given resource by setting 'bias'
    * to use this routing, you must use 'Route53 traffic flow'
- Multivalue Answer Routing
    - lets you configure Route53 to return multiple values, such as IP addresses for your web, in response to DNS queries. You can specify multiple values for almost any record, but multivalue answer routing also lets you check the health of each resource, so Route 53returns only values for healthy resources
    * similar to simple routing but allows to put health checks on each record set