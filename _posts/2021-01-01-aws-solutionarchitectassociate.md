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

S3 Lifecyle management
-	Automates moving your objects between the different storage tiers.
-	Can be used in conjunction with versioning
-	Can be applied to current versions and previous versions

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
-	Use multipart uploads to increase performance when uploading files to S3
-	Should be used for any files over 100MB and must be used for any file over 5GB
-	Use S3 byte-range fetches to increase performance when downloading files to S3

S3 Select & Glacier Select 
-	Remember that S3 Select is used to retrieve only a subset of data from an object by using simple SQL expressions
-	Get data by rows or columns using simple SQL expressions
-	Save money on data transfer and increase speed
Some Best Practices With AWS Organizations
-	Always enable multi-factor authentication on root account
-	Always use a strong and complex password on root account
-	Paying account should be used for billing purposes only. Do not deploy resources into the paying account
-	Enable/Disable AWS services using Service Control Policies (SCP) either on OU or on individual accounts

Sharing S3 Buckets Between Accounts
-	Using Buckets Policies & IAM (applies across the entire bucket), Programmactic Access Only
-	Using Bucket ACLs & IAM (individual objects), Programmatic Access Only
-	Cross-account IAM Roles, Programmatic AND Console access. (create a role with a policy, assign it to a user, login to the user and switch)

Cross Region Replication
-	Versioning must be enabled on both the source and destination buckets
-	Files in an existing bucket are not replicated automatically (old ones won’t be replicated)
-	All subsequent updated files will be replicated automatically
-	Delete markers are not replicated
-	Deleting individual versions or delete markers will not be replicated
-	Understand what Cross Region Replication is at a high level


DataSync Overview
- Used to move large amount of data from on-premises to AWS
- Used with NFS- and SMB-compatible file systems
- Replication can be done hourly, daily, or weekly
- Install the DataSync agen to start the replication 
- Can be used to replicate EFS to EFS

CloudFront Overview
- Edge Location: This is the location where content will be cached. This is separate to an AWS Region/AZ
- Origin : This is the origin of all the files that the CDN will distribute. This can be either an S3 bucket, an EC2 Instance, an Elastic load Balancer, or Route53
- Distribution: This is the name given the CDN which consists of a collection of Edge Locations
- Web Distribution: Typically used for Websites
- RTMP: Used for media streaming
- Edge locations are not just READ only : you can write to them too (ie. put an object onto them)
- Objects are cached for the life of the TTL(Time To Live)
- You can clear cached objects, but you will be charged.

CloudFront Signed URLs and Cookies
- Use signed URLs/cookies when you want to secure content so that only the people you authorize are able to access it
- A signed URL is for individual files. 1 file = 1 URL
- A signed cookie is for multiple files. 1 cookie = multiple files
- if your origin is EC2, then use CloudFront

Snowball
- use when moving large amount of data into the AWS cloud
- snowvall can import S3, export from S3

Storage Gateway
- is a physical device, that connects legacy into S3
- File Gateway: for flat files, stored directly on S3
- Volumn Gateway
    - Stored Volumns: entire dataset is stored on site and is asynchronously backed up to S3
    - Cached Volumns: entire dataset is stored on S3 and the most frequently accessed data is cached on site
- Gateway Virtual Tape Library


Athena vs Macie

- Athena
    - is an interatice query service
    - allows you to query data located in S3 using standard SQL
    - Serverless
    - commonly used to analyse log data stored in S3
- Macie
    - uses AI to analyze data in S3 and helps identify PII(Personally Identifiable Information)
    - can also be used to analyse CloudTrail logs for suspicious API activity
    - includes Dashboards, reports and alerting
    - great for PCI-DSS compliance and preventing ID theft
