# EC2 FUNDAMENTALS

- **EC2** stands for Elastic Cloud Compute.

---

## EC2 Sizing And Configurations Options

- Operating System (OS): Linux, Windows or MAC OS.
- How much compute power and Cores (CPU).
- How much RAM.
- How much storage space? i.e Network attached(EBS & EFS) or Hardware attched (EC2 Instances Store).
- Network Card; speed of card, Public IP address.
- Firewall rules: security group
- Bootstrap script (Configure at first launch): EC2 User Data.

## Instances are divided into 7 categories

1. General purpose
2. Memory Optimized
3. Accelerated Computing
4. Storage Optimized
5. Instance Features
6. Compute Optimized

---

## Instance naming convention

An example is **m5.2xlarge** where

**m** is Instance Class,

**5** is Generation(AWS improves over time)

**2xlarge** size within the instance class

## Introduction to Security Groups

- Are the fundamentals of network security in AWS.
- They control how traffic is allowed inot or out of our EC2 instances.
- Only contain **"Allow"** rules
- Act as a firewall on our EC2 instances.
- They regulate:
  - Access to ports
  - Authorised IP ranges - IPV4 and IPV6
  - Control both inbound and outbound network.

![ec2-security-group](./images/ec2-sg.png)

## EC2 Instances Purchasing Options

- **On-Demand Instances:**

Ideal for short workload, Predictable pricing and it's pay per second.

- **Reserved (1 & 3 yrs):**
  - **Reserved Instances:** Good for workloads that span across a long period.
  - **Convertible Reserved Instances:** Long workloads with flexible instances.
- **Savings Plans (1 & 3 yrs):**

Commitment to an amount of usage, can be ideal for long workloads too.

- **Spot Instances:**

Used for short workloads, cheap but can loose it at any time.

- **Dedicated Hosts:**

Book an entire physical server and control how instances are placed on these physical servers.

- **Dedicated Instances:**

No other customers will share your hardware.

- **EC2 On Demand:**

   You pay for what you use:
  - Linux and Windows - Billing per second , after the first minute.
  - All other OS billing is per hour.

- Has the highest cost but no upfront payment and no long term commitment.

- Recommended for short-term uninterupted workloads, where you can't predict hoe the application will behave.

## Placement Groups

Can be used for control over the EC2 instance placement strategy.

 These strategy can be defined using placement groups.
 Some of these strategies include;

- **Cluster:** Clusters instances into a low-latency group in a single AZ. On the same Rack. Node-to-Node communication that is typical of HPC applications.

- **Spread:** Spreads instances across the underlying hardware i.e 7 isntances per group per AZ and it's good for critical applications. Different hardware. can spread across multiple AZs.

- **Partition:** Spreads instances across many different partitions (which rely on different sets of racks) within an AZ or multiple AZs. Scales to 100s of EC2 instances per group. (Hadoop, Cassandra, Kafka). Distrbuted and replicated workloads. 7 partitions per AZ.

## Elastic Network Interface (ENI)

- Logical component in a VPC that represents a **vitual network card**.
- Used outside of EC2 instances.
- Gives EC2 instances access to the network

### ENI Attributes

- Have one Primary IPv4, one or more secondary IPv4.
- One Elastic IPv4 per private IPv4.
- One public IPv4.
- One or more security groups.
- Mac address.
- Bound to specific AZ
- Can create ENIs independently from and attach them on the fly(move them) on EC2 instances for failover.

## EC2 Hibernate

- The in-memory (RAM) state is preserved.
- The instance boot is much faster(the OS is not stopped/restarted).
- Underthe hood, the RAM state is written to a file in the root EBS volume.
- The root EBS volume must be encrypted.

- Use cases:
  - long-runnig processing
  - Saving the RAM state
  - Services that take time to initialize

### EC2 Hibernate - GOOD to KNOW

- **supported instances families:** C3,C4,C5,I3,M3,M4,R3,R4,T2,T3.
- **Instance RAM Size:** must be less than 150GB
- **Instance Size:** Not supported for bare metal instances.
- **AMIs:** Amazon linux2, linux AMI, Ubuntu, RHEL, CentOS and Windows.
- **Root Volume:** Must be EBS, encrypted, not instance store and large, Available for On-demand, Reserved and spot instances.
- An isntance cannot be hibernated more than 60days.

## EC2 Instance Storage

### What is EBS(Elastic Block Store) Volume?

- EBS (Elastic Block Store) Volume is a network drive you can attach to your instances while the run.
- Allows your instance to persist data even after termination.
- Bound to specific AZ.
- Likened to a "network USB stick".
- Free tier: 30GB general type storage (ssd) or magnetic.
- It's a network drive hence
  - uses network to communicate, wh means ther might be some latency.
  - can be detached from one instance and attached to another relatively quickly in case of fail over.
- Have a provisioned capacity (size in GBs and IOPS)
  - get billed for all the provisoned capacity
  - you can increase the capacity of the drive over time.

## EBS Snapshots

- make a backup (snapshot) of your EBS volume at any given point in time.
- Not necessary to detach volume before doing a snapshot but it's recommended.
- Can copy snapshots accross AZ or Region.

### EBS Snapshots Features

- EBS snapshot archive
  - move snapshot to an "archive tier" that is 75% cheaper
  - takes about 24 to 72 hrs for restoring the archive

- Recycle Bin for EBS Snapshots
  - setup rules to retain deleted snapshots so you can recover them after an accidental deletion
  - specify retention(from 1 day to 1 yr)

- Fast Snapshot Restore
  - Force full initialization of snapshot to have no latency on the first use. Costly ($$)

## AMI (Amazon Machine Image)

- AMI is customization of EC2 instance
  - you add your own software, OS, monitoring
  - faster boot/configuration time because all your software is pre-packaged

- AMIs are built for specific regions (and can be copied from one region to another)
- instances can be launched from
  - Public AMI: provided by AWS
  - Owned AMI: managed and maintained by yourself
  - AWS marketplace AMI

## EC2 Instance Store

- EBS volumes are network drives with good but limited performance.
- If you need a high-performance hardware disk, use EC2 Instance Store.
- Better I/O performance
- EC2 Instance Store lose thier storage if they're stopped (ephemeral)
- Good for buffer/ cache/ scratch data/ temporary content.

### EBS Volume types

- EBS volumes come in 6types
  - **gp2/gp3 (SSD)**: general purpose SSD volume. Balancesprice and performance for a wide variety of workloads.
  - **io1/io2 (SSD)**: Highest-performance SSD volume for mission-critical low-latency or high-throughput workloads.
  - **st1 (HDD)**: Low cost HDD volume designed for frequently accessed, throughput-intensive workloads.
  - **sc1 (HDD)**: Lowest cost HDD volume designed for less frequently accessed workloads.

- EBS volumes are characterized in size | throughput | IOPS (I/O Ops Per Sec)

## EBS multi-Attach - io1/io2 family

- Attach same EBS volume to multiple EC2 instances in the same AZ
- Each instance have full read and write permissions to the high-performance volume.
- Use Case;
  - Achieve higher application availability in clustered linux applications (ex: Teradata)
  - Applications must manage concurrent write operations.
- Up to 16 EC2 instances at a time.
- Must use file system that's cluster-aware (not XFS,EXT4 etc)

## EBS Encryption

- When you encrypt your EBS volume, you get the following;
  - Data at rest is encrypted inside the volume.
  - All data in flight between the instance and the volume is encrpyted.
  - All snapshots are encrypted
  - All volumes created from the snapshots are encrypted too.
- Encryption and Decryption are handled transparently(hence you have nothing to do).
- Ecryption has minimal impact on latency
EBS encryption leverages keys from KMS (AES-256)
- Copying an unecrypted snapshot allows encryption.
- Snapshots of encrypted volumes are encrypted.

### How can you encrypt an Unecrypted EBS Volume?

- Create an EBS snapshot of the volume.
- Encrypt the EBS snapshot (using copy)
- Create new EBS volume from the snapshot(the volume will be encrypted)
- Attach encrypted volume to the original instance.

## Amazon EFS (Elastic File System)

- Managed Network File System (NFS) that can be mounted on many EC2
- EFS works with EC2 instances in multi-AZ
- Highly available, scalable, expensive (3x gp2), pay per use.

- Use cases are;
  - content management, web serving, data sharing, wordpress
- Uses NFSv4.1 protocol
- Uses security group to control access to EFS.
- Compatible with Linux based AMI (not windows)
- Encryption at rest uaing KMS.

- POSIX file system (~Linux) that has the standard file API.
- File system scales automatically, pay-per-use, no capacity planning.

## Scalabilty and High Availability

- Scalability means that an application/system can handle greater workloads by adapting.
- There are 2 kinds;
  - Vertical Scalability
  - Horizontal Scalability (Elasticity)

- **Vertical Scalabilty;** means increasing the size of the instance.
- Example is scaling your instance from t2.micro to t3.large
- Common for non-distributed systems such as database. RDS and ElastiCache can scale vertically too.
- Has hardware limits.

- **Horizontal Scalabilty;** meansincreasing the number of instances/systems for your application.
- Horizontal scaling implies distributed systems and common with modern web application.

- **High Availabity;** Usually goes hand in hand with horizontal scaling.
- High availability means running your application/system in at least 2 datacenters(==AZ)
- Goal is to survive datacenter loss.
- HA can be passive(RDS multi-AZ)
- HA can be active(for horizontal scaling)

## Load Balancers

- LBs are servers that forward traffic to multiple servers.

### Why use LBs?

- Spread load across multiple downstream isntances.
- Expose a single point of acess(DNS) to your application
- Seamlessly handle failures of downstream instances
- Do regular healthchecks to your instances
- Provide SSl termination(HTTPS) for your website
- Enforce stickiness with cookies
- High availability across zones
- Seperate public traffic from private traffic.

## Why use ELB(Elastic Load Balancer)?

- An ELB is a managed Load Balancer.
  - AWS guarantees that it will be working.
  - AWS takes care of upgrades, maintainance, HA.
  - AWS provides only a few configuration knobs.
- It's integrated with many AWS offerings/services like EC2, EC2 Auto Scaling Groups, Amazon ECS, AWS cert Manager(ACM), CloudWatch, Route53, AWS WAF, AWS Global Accelerator.

## Types of Load Balancers on AWS

- **classic Load Balancer** (v1 - old generation) from 2009 -CLB
- **Application Load Balancer** (v2 -new generation) - 2016 - ALB
- **Network Load Balancer** (v2 -new genration) -2017 - NLB
- **Gateway Load Balancer** (Operates at layer3 network layer -IP protocol) - 2020 - GWLB

## Application  Load Balancer (v2)

- ALB is a layer 7(HTTP)
- Load balancing to multiple HTTP applications across machines(target groups)
- Can be used to load balance to multiple applications on the same machine(eg:containers like ECS)
- Support for HTTP/2 and WebSocket
- Supports redirect (HTTP to HTTPS at the load balancer level)
- Routing tables to different target groups:
  - Routing based on path in URL (example.com/users & example.com/posts)
  - Routing based on hostname in URL(one.example.com & other.example.com)
  - Routing based on Query String, Headers(example.com/users?id=123&order=false)

- ALBs are great for microservices & container-based application(eg; Docker & ECS)
- Has port mapping feature to redirect to a dynamic port in ECS

![ALB-Image](/IAM%20and%20AWS%20CLI/ALB.png)

### ALB Target Groups

- EC2 Instances(managed by ASG) - HTTP
- ECS tasks (managed by ECS itself) - HTTP
- Lambda functions - HTTP request is translated into a JSON event
- IP Addresses - must be private IPs.
- ALB can route to multiple target groups
- Health checks are at the target group level.

## Network load Balancer

- It's a layer 4
  - forward TCP and UDP traffic to your instances
  - High performance compared to the ALB and handles millions of requests per second.
  - less latency ~100ms(vs 400ms for ALB)
- NLB has one static IP per AZ and supports assinging Elastic IP(helps in whitelisting specific IP)

### NLB target groups

- EC2 instances
- IP addresses
- Application load balancer
- Health checks support the TCP, HTTP, and HTTPS protocols

## Gateway Load Balancer(GWLB)

- Deploy, scale and manage a fleet of 3rd party network virtual appliances in AWS
- Examples include: Firewalls, Intrusion Detection and prevention systems, Deep Packet Inspection Systems, payload manipulation
- Operates at layer 3(Network Layer) IP Packets.
- Combines the following Functions:
  - Transparent Network Gateway - single entry/exit for all traffic
  - Load Balancer - Distributes traffic to your virtual appliances.
- Uses GENEVE protocol on port 6081

### GWLB target groups

- EC2 instances
- IP addresses which must be private IPs

## Sticky Sessions (Session Affinity)

- Enables the same client to always be redirected to the same instance behind the load balancer
- Works for network load balancers(works without cookies) and Application Load balancers.
- The "cookie" ised for stickiness has an expiration date you control.
- Enabling stickiness may bring imbalance to the load over the backend EC2 instances

### Sticky Sessions - Cookie Names

- **Application-based Cookies**

- Custom cookie
  - generated by the target
  - can include any custom attributes required by the application
  - Cookie name must be specified individually for each target group
  - Can't use AWSALB, AWSAAPP, or AWSALBTG (rederved for use by ELB)
- Application cookie
  - generated by the load Balancer
  - Cookie name is AWSALBAPP

- **Duration-based Cookies**
- Cookie generated by Load balancer
- Cookie name is AWSALB for ALB, AWSELB for CLB

## Elastic Load Balancer - SSL(secure sockets layer) Certificates

- An SSL Certificate allows traffic between your clients and load balancer to be encrypted in transit(in-flight encryption)
- **SSL** refers to Secure Sockets Layer, used to encrypt connections
- **TLS** refers to Transport Layer Security, which is newer version.
- Public SSL certs are issued by certificate authorities(CA) for example, comodo,symantec, Godaddy, GlobalSign, Digicert, Letsencrypt etc.
- Load balancer uses an X.509 certificate (SSL/TLS server cert.)

### Server Name Indication (SNI)

- Solves the problem of loading multiple SSL certificates onto one web server(to serve multiple websites.)
- It's a newer protocol that requires the client to indicate the hostname of the server in the initial SSL handshake. The server will find the certificate, or return the default one.

**Note:**

- *Only works for ALB & NLB(newer generation), cloudfront.*

## Connection Draining

### - Feature Naming

- Connection Draining for CLB
- Deregistration Delay - for ALB & NLB

- Time to complete "in-flight requests" while the instance is de-registered or unhealthy.
- Stops sending new requests to the EC2 instance which is deregistering.
- Between 1 to 3600 seconds (default: 300seconds)

## What's an Auto Scaling Group?

- The use of an ASG is to :
  - Scale out(increase) EC2 instances to match increased workloads
  - Scale in(decrease) EC2 instances to match decreased workloads
  - Ensure min and max numbers of EC2 instances is running.
  - Automatically register new instances to a load balancer
  - Re-create an EC2 instance in case a previous one is terminated because it was unhealthy.
- ASGs are free(only pay for the underlying instances)

### ASG Attributes

- A lauch Template is what is needed to automatically spin up new instances in your ASG
- It comprises of;
  - AMI + Instance Type
  - EC2 user Data
  - EBS Volumes
  - Security Groups
  - SSH Key Pair
  - IAM Roles for your EC2 Instances
  - Network + Subnets Information
  - Load Balancer Information
- Has a min and max size
- Scaling Policies using cloudwatch alarms

## ASG - Dynamic Scaling Policies

- Target tracjing scaling
  - Most simple and easy to setup
  - Example; I want the average ASG CPU to stay at around 40%
- Simple/Step Scaling
  - when cloudwatch alarm is triggered(example cpu > 70%), then add 2 units or (CPU < 30%), then remove 1 instance.
- Scheduled Actions
  - Anticipate a scaling based on known usage patterns
- Predictive scaling
  - Using forcasting

## AWS RDS Overview

- RDS = Relational Database Service
- It's a managed DB service for DB use SQL as a query language
- It allows you to create databases in the cloud that are managed by AWS. These include;
  - Postgres
  - MySQL
  - MariaDB
  - Oracle
  Microsoft SQL Server
  - Aurora (AWS Proprietary database)

### Advantage of using AWS RDS vs Self-Deployed RDS on EC2 or VM

- RDS is a managed service:
  - automated provisioning and OS patching.
  - Continuous backups and restore to specific timestamps (point in time restore)
  - Monitoring dashboards
  - Read replicas for improved read performance
  - Multi AZ setup for disaster recovery(DR)
  - Maintainance windows for upgrades
  - Scaling capability (both vertical and horizontal)
  - Storage backed by EBS (gp2 or io1)
- BUT you can't SSH into your instances

### RDS - Storage Auto Scaling

- Helps increase storage on your RDS DB instance dynamically
- When RDS detects you are running out of free DB storage, it scales automatically.
- You need to set a max storage threshold

## RDS Read Replicas for read scalability

- Read replicas helps to scale your reads.
- can have up to 15 read replicas ehich can be within same AZ, cross AZ and cross region
- Replication is ASYNC, so reads are eventually consistent.
- Read repicas can be promoted to their own DB
- Applications must update the connection string to leverage read replicas.

![rds-read-replicas](/IAM%20and%20AWS%20CLI/RSD-replica.png)

## RDS Read Replicas - Network Cost

- in AWS, there's always a network cost when data goes from one AZ to another
- within the same region, AWS charges no fee for your read replicas in different AZs.
- There's a fee for read replicas in different AZs found on different regions.

### RDS Multi-AZ (Disaster Recovery)

- SYNC replication is established in a standby DB in a different AZ.
- One DNS name for both SB instances so that in case of a disaster, there's an automatic failover.
- Increase availability
- Failover in case of loss of AZ, loss of network, instance or storage failure.
- Not used for scaling

![rds-read-replicas](/IAM%20and%20AWS%20CLI/RDS-Multi.png)

### RDS Custom

Managed Oracle and Microsoft SQL server with OS and database customization

- You can access the underlying database and OS so you can
  - Configure settings
  - Install patches
  - Enable native features
  - Access the underlying EC2 instance using SSH or SSM Session Manager

- De-activate Automation Mode to perform your customization, better take snapshots before.

## Amazon Aurora

- Aurora is a proprietary technology from AWS(not open-sourced)
- Postgres and MySQL are now supported as Aurora DB(means your drivers will work as if Aurora was a Postgres or MySQL db).
- It's aws cloud optimized and claims 5x performance improvement over MySQL, 3x performance over Postgres on RDS.
- Its storage automatically grows in increments of 10GB up to 128 TB.
- Aurora can have up to 15 replicas and the replication process is faster than MySQL (sub 10 ms replica lag)
- Instantaneous failover. It's HA native.
- It's cost more than RDS (20% more) - but is more efficient

### Aurora HA an Read Scaling

- 6 copies of your data across 6 AZs
  - 4 copies out of 6 need for writes
  - 3 outof 6 copies needed for reads
  - Self healing with peer-to-peer replication
  - Storage is striped across 100s of volumes.
- One Aurora instance takes writes (master)
- Automated failover for master in less than 30 secs
- Master + up to 15 Aurora read replicas serve reads
- Supports cross region replication.

### Aurora DB Cluster

![aurora-cluster](/IAM%20and%20AWS%20CLI/aurora-cluster.png)

## Aurora Replicas

### - Auto Scaling

- Writer endpoint attached to the primary DB
- Reader endpoint on the read replicas, which can be extended as the read replicas can be scaled out.

![aurora-asg](/IAM%20and%20AWS%20CLI/aurora-asg.png)

### - Custom Endpoints

- Usually when you have different types of Read Replicas.
- You can define a subset of your Aurora Instances as a Custom Endpoint
  - Example willbe to  run analytical queries on specific replicas.
- The reader endpoint is genereally not used after defing Custom Endpoints

![custom-endpoint](/IAM%20and%20AWS%20CLI/custom-endpoint.png)

## Aurora Serverless

- Automated database instantiation and auto-scaling based on actual usage.
- Good for infrequent, intermittent or unpredictable workloads
- No capacity planing needed and pay per second can be more cost effective.

![aurora-serverless](/IAM%20and%20AWS%20CLI/aurora-serverless.png)

## Global Aurora

- Aurora Cross Region Read Replicas:
  - Useful for disaster recovery
  - Simple to put in place
- Aurora Global Database (recommended):
  - 1 Primary region (read/write)
  - Up to 5 secondary (read-only) regions, replication lag is less than 1 second
  - Up to 16 Read Replicas per secondary region
  - Helps in decreasing latency
  - Promoting another region (for disaster recovery) has an RTO < 1 min
  - Typical cross-region replication taks less than 1 second

### Aurora Machine Learning

- Enables you to add ML-based predictions to your applications via SQL
- Simple, optimized, and secure integration between Aurora and AWS ML services
- Supported services are
  - AWS sagemaker (use with any ML model)
  - Amazon Comprehend (for sentiment analysis)
- You don't need to have ML experience
- Use cases are:
  - fraud detection, ads targeting, sentiment analysis, product recommendations.

## Backups

### RDS Backups

- Automated backups:
  - Daily full backup of database (during the backup window)
  - Transactions logs are backed-up by RDS every 5mins.
  - Ability to restore to any point in time (from oldest backup to 5 min ago)
  - 1 to 35 days of retention, set 0 to disable automated backups
- Manual DB Snapshots
  - Manually triggered by the user
  - Retention of backup for as long as you want
- Trick: in a stopped RDS database, you will pay for storage. fi you plan on stopping it for a long time, you should snapshot and restore instead.

### Aurora Backups

- Automated backups
  - 1 to 35 days (cannot be disabled)
  - point-in-time recovery in that timeframe

- Manual DB Snapshots
  - Manually triggered by user
  - Retentions of backup for as long as you want.

## RDS & Aurora Restore Options

- Restoring a RDS / Aurora backup or a snapshot creates a new db

- **Restoring MySQL RDS database from S3**
  - Create a backup of your on-premises db
  - Store it on Amazon S3
  - Restore the backup file onto a new RDS instance running MySQL

- **Restoring MySQL Aurora cluster from S3**
  - Create a backup of your on-prem db using Percona CtraBackup
  - Store the backup file on Amazon S3
  - Restore the backup file onto a new Aurora cluster running MySQL

## Auro Database Cloning

- You can create a new Aurora DB cluster form an existing one.
- Faster than snapshot and restore
- Uses *copy-on-write* protocol
  - Initially, the new DB cluster uses th same data volume as the original DB cluster (fast and efficient - no copying is needed)
  - when updates are made to the new DB cluster data, then additonal storage is allocated and the data is copied to be seperated
- Very fast and cost-effective
- Useful to create a "staging" database from a "production" db without impacting the production database.

## RDS & Aurora Security

- **At-rest Encryption:**
  - DB master and replicas encryption using AWS KMS(must be defined at launch time).
  - If the master is not encrypted, the read replicas cannot be encrypted.
  - But to ecrypt an un-encrypted DB, you have to go through a DB snapshot and restore as encrypted.
- **In-flight encryption:**
  - TLS-ready by default, use the AWS TLS root certificates client-side.
- **IAM Authentication:**
  - Can make use if IAM roles to login or connect to your database (instead of username/passwords).
- **Security Groups:**
  - Control network access to your RDS / Aurora DB
- **No SSH Access:**
  - Because RDS and Aurora are managed services, it doesn't provide ssh access except on *RDS Custom service*
- **Audit Logs can be enabled** and sent to cloudwatch logs for longer retention

## Amazon RDS Proxy

- Fully managed DB proxy for RDS
- Allows apps to pool and share DB connections established with the DB.
- Improving db efficiency by reducing the stress on database resources (eg CPU, RAM) and minimize open connections(and timeouts.)
- Serverless, autoscaling, highly available (multi-AZ)
- Reduced RDS and Aurora failover time by up to 66%
- Supports RDS (MySQL, PostgreSQL, MariaDB, MS SQL server) and Aurora (MySQL, PostgeSQL)
- No code changes required for most apps
- Enforce IAM Authentication for DB and securely store credentials in AWS Secrets Manager.
- RDS Proxy is never publicly accessible (must be accessed from VPC).

## ElastiCache Overview

- Caches are in-memory databases with really high performance, low latency.
- ElastiCache is to get managed Redis or Memcached
- Helps reduce load off db for read intensive workloads and helps make your applications stateless.
- AWS is responsible fo the OS patches, maintainance, optimization, setup, configuration, monitoring,failure recovery and backups.
- Using ElastiCache involves heavy application code changes.

### ElastiCache Solution Architechture - DB Cache

- Application queries ElastiCache, if not available, gets from RDS and store in ElastiCache.
- Cache must have an invalidation strategy to make sure only the most current data is used there.

![db-cache](/IAM%20and%20AWS%20CLI/db-cache.png)

#### ElastiCache Solution Architechture - User Session Store

- user logs into any of your application
- The app writes the session data into  ElastiCache
- The user hits another instance of your app.
- The instance retrieves the data and the user is already logged in.

![user-session-cache](/IAM%20and%20AWS%20CLI/user-session-cache.png)

## ElastiCache - Cache Security

- ElastiCache supports IAM Authentication for Redis
- IAM policies on ElastiCache are only used for AWS API-level security.
- **Redis AUTH**
  - You can set a "password/token" when you create a Redis cluster
  - Extra level  of security for your cache (on top of security group)
  - Support SSL in flight encryption
- **Memcached**
  - Supports SASL-based authentication (advanced)

## Patterns of ElastiCache

- **Lazy loading:** All the read data is cached, data can be stale in store
- **Write through:** Adds or update data in the cache when wriiten to a DB( no stale data)
- **Session Store:** Stores temporary session data in a cache(using TTL feature)

## AWS Route53

### DNS Terminologies

- **Domain Registrar:** Godaddy, Amazon Route 53...
- **DNS Records:** A, AAAA, CNAME, NS, ALIAS...
- **Zone File:** contains our DNS records
- **Name Server:** Resolves DNS queries (Authoritative or non-Authoritative)
- **Top Level Domain(TLD):** .com, .us, .in, .gov, .org, ...
- **Second Level Domain(SLD):** amazon.com, google.com,...

![fqdn](/IAM%20and%20AWS%20CLI/domain.png)

## How Does DNS Server Work?

Below is a visual image of how DNS work.

![dns-server](/IAM%20and%20AWS%20CLI/dns-server.png)

## Route 53 - Records TTL (Time To Live)

- We don't want to query the DNS too often. So the client will cache the result of a query for the TTL of the record.

- Higher TTL eg 24hr
  - Less traffic on Route 53
  - Possibly outdated records

- Lower TTL eg; 60secs
  - More traffic on route 53 ($$)
  - Records are outdated for less time
  - Easy to change records

- **Except for Alias records, TTL is mandatory for each DNS record.**

## CNAME vs Alias

AWS resources (LB, Cloudfrint) expose an AWS hostname: *lb-1234.us-east-2.elb.amazonaws.com* and you want *myapp.mydomamain.com*

- CNAME:
  - points a hostname to any other hostname. eg: app.mydomain.com => web.blabla.com .
  - **ONLY FOR NON ROOT DOMAIN (aka.something.mydomain.com)**

- Alias:
  - Points a hostname to an AWS Resource (app.mydomain.com => blalbla.amazonaws.com)
  - Works for both ROOT DOMAIN and NON ROOT DOMAIN (aka mydomain.com)
  - Free of charge
  - Native healthcare capability within them.

### Alias Records

- Maps a hostname to an AWS resource
- An extension to DNS functionality
- Automatically recognizes changes in the resource's IP addresses
- Unlike CNAME, it can be used for the top node of a DNS namespace (Zone Apex)eg
example.com
- Alias Record is always of type A/AAAA for aws resources (IPV4 / IPV6)
- You can't set the TTL

### Alias Records Targets

- Elastic Load Balancers
- CloudFront Distributions
- API Gateway
- Elastic Beanstalk environments
- S3 websites
- VPC interface Endpoints
- Global Accelerator
- Route 53 record in the same hosted zone

## Routing Policies

- **Simple Routing;**
  
  - Typically route traffic to a single resource
  - can specify multiple values in the same record
  - if multiple values are returned, a random one is chosen by the client.
  - when alias is enabled, specify ony one AWS resource
  - Can't be associated with health checks.

- **Weighted Routing;**
  - control number or % of request that go to a particular resource.
  - DNS records must have the same name and type
  - can be associated with health checks

  ![weight-routing](/IAM%20and%20AWS%20CLI/weight-routing.png)

- **Latency-based Routing;**
  - Redirect to the resource that has the least latency close to us
  - Super helpful when latency for users is priority
  - **Latency is based on traffic between users and AWS regions**
  - Can be combined with health checks and has failover capacity.

### Route 53 - Health Checks

- HTTP Health checks are only for public resources
- Health Check => Automated DNS Failover: there are different kinds
  - Health checks that monitor an endpoint(application, server, other AWS resources)
  - Health checks that monitor other health checks (Calculated Health Checks)
  - Health checks that monitors cloudwatch alarms (full control !!) eg: throttles of DynamoDB, alarms on RDS, custom metrics, (helpful for private resources)
- Health Checks are integrated with CloudWatch mtrics.

![health-check](/IAM%20and%20AWS%20CLI/health-check.png)

## Routing Policies

- **Failover (Active-Passive)**
![failover](/IAM%20and%20AWS%20CLI/failover.png)

- **Geolocation**
  - Different from latency-based.
  - **This routing is based on user location**
  - Specify location by continent, country or by US state(if there's an overlapping, more precise location selected)
  - Should create a "Default" record (in case there's no match on location)
  - Use cases; website localization,restrict content distribution, load balancing.
  - Can be associated with healthchecks

- **Geoproximity Routing**
  - Allows you to route traffic to your resources based on the geographic location of users and resources.
  - ability to shift more traffic to resources based on the defined bias
  - To change the size of the geopgraphic region, specify bias values:
    - To expand (1 to 99) - more traffic to the resource
    - To shrink (1 to 99) - Less traffic to the resource
  - Resources can be:
    - AWS Resources (specify AWS region)
    - Non-AWS Resources (specify Latitude and Longitude)
  - You must use Route 53 Traffic Flow (advanced) to use this feature

- **IP-based Routing**
  - Routing is based on client IP addresses
  - You provide a list of CIDRs for your clients and the corresponding endpoints/locations (user-IP-to-endpoint mappings)
  - Use case; Optimize performance and reduce network costs eg: route end users from a particular ISP to a specific endpoint

  ![ip-based-routing](/IAM%20and%20AWS%20CLI/ip-based-routing.png)

- **Multi Value routing**
  - Use when routing traffic to multiple resources and can be associated with health-checks
  - Up to 8  healthy records are returned for each multi-value query and route 53 return multiple values/resources
  - **Not a substitude for having an ELB.**

## Elastic Beanstalk

- Developer centric view of deploying application on AWS.
- Uses all the components we've seen before, such as; EC2, ASG, ELB, RDS etc.
- Also a managed service.
  - Automatically handles capacity provisioning, load balancing, scaling, application health monitoring, instance configuration
  - Just the application code is the responsiblity of the developer.
- We Still have full control over the configuration
- Beanstalk is free but you pay for the underlying instances.

### Elastic Beanstalk Components

- **Application:** Collection of Elastic Beanstalk components (environments, versions, configurations, ...)
- **Application Version:** An iteration of your application code
- **Environment**
  - Collection of AWS resources running an applciation version(only one applciation version at a time)
  - *Tiers:* Web Server Environment Tier & Worker Environment Tier
  - One can also create multiple environments(dev,test,prod,..)

![beanstalk](/IAM%20and%20AWS%20CLI/beanstalk.png)

## Amazon S3

### Use cases

- For backup and storage
- Disaster Recovery
- Archive
- Hybrid Cloud Storage
- Application Hosting
- Media Hosting
- Data Lakes & big data analytics
- Software delivery

### Amazon S3 Buckets

- S3 allows you to store objects(files) in buckets (directories)
- S3 Buckets must have a **globally unique name** (across all regions and all accounts)
- Looks like a global service but buckets are created in regions and are defined at the region level
- Naming Convention
  - No uppercase, No underscore
  - 3 to 63 characters long
  - Not an IP
  - Must start with lowercase letter or number
  - Must NOT start with prefix **xn--**
  - Must NOT end with end with suffix **-s3alias**

### Amazon S3 Objects

- Object values are the content of the body:
  - Max Object size is 5TB(5000GB)
  - If uploading more than 5GB, must use "multi-part upload"

### Amazon S3 Security

- **User-based:**
  - **IAM Roles** determine which API calls should be allowed for a specific user from IAM.
- **Resource-Based:**
  - **Bucket Policies** - bucket wide rules from the s3 console(allows cross account)
  - **Object Access Control List(ACL)** - fine grain(can be disabled)
  - **Bucket Access Control List(ACL)** - less common (can be disabled)

- **Encryption:**
  - S3 buckets can be encrypted using encryption keys.

## S3 Storage Classes

1. **Amazon S3 Standard -General Purpose:**
   - 99.99% availability
   - Used for frequenntly accessed data
   - Low latency and high throughput
   - Sustain 2 concurrent facility failures
   - Use case is; Big data analytics, Mobile & gaming applications, content distibution.

2. **Amazon S3 Standard-infrequent access (IA)**
    - For data less frequently accessed but requires rapid access when needed.
    - Lower cost than s3 standard but cost on retrieval.
    - 99.99% availability
    - Use case; Disaster Recovery(DR) snd backups.

3. **Amazon S3 One Zone-Infrequent Access**
    - High durability (99.999%) in a single AZ, data lost when AZ is destroyed
    - 99.5% availability
    - Use case; storing secondary backup copies of on-premise data, or data you can recreate.

4. **Amazon S3 Glacier Instant Retrieval**
    - Millisecond retrieval, great for data accessed once a quarter
    - Minimum storage duration of 90 days

5. **Amazon S3 Glacier Flexible Retrieval(formerly amazon s3 glacier)**
    - Expedited (1 to 5min), standard (3 to 5hrs), Bulk(5 to 12hrs) - free
    - Minimum storage duration of 90 days.

6. **Amazon S3 Glacier Deep Archive**
    - Good for long term storage.
    - Standard(12 hrs), Bulk (48 hrs)

7. **Amazon S3 Intelligent Tiering**
    - Small monthly monitoring and auto-tiering fee
    - Moves objects automatically between access tiers based on usage
    - There are no retrieval charges in S3 intelligent-Tiering
        - Frequent Access tier(automatic): default tier
        - Infrequent Access tier(automatic): objects not accessed for 30days
        - Archieve Instant Access tier(automatic): objects not access for 90 days
        - Archieve Access tier(optional): configurable from 90 days to 700+ days

        ![s3 storage classes](/IAM%20and%20AWS%20CLI/s3-storage.png)

- **You can use s3 lifecycle configurations to move objects between the classes above.**

### Amazon S3 LifeCylce Rules

- **Transition Actions**
  - configure objects to transition to another storage class
    - Move object to standard IA class 60 days after creation
    - Move to Glacier for archiving after 6months

- **Expiration Actions**
  - Configure Objects to expire(delete) after a certain period
    - Access log files can be set to delete after a 365 days
    - Can be used to delete old versions of files(if versioning is enabled)
    - can be used to delete incomplete Multi-Part uploads
  - Rules can be created for certain prfix (example:s3://mybucket/mp3/*)
  - Rules can be created for certain object Tags (example:Department:Finance)

### S3 Requester Pays

- We know that in general, buvket owners pay for all s3 storage and data transfer cost associated with their bucket.
- **With Requester Pays Buckets**, the requester instead of the owner pays for the cost of the request and the data downloadfrom the bucket.
- Helpful when you want to share large datasets with other accounts.
- The requester must be authenticated in AWS.(can't be anonymous)

## Amazon S3 - Object Encryption

Can encrypt objects in s3 using 4 methods.

- **Server-side Encryption (SSE);**
  - Server-side encryption with amazon s3-managed Keys(SSE-S3) and it's enabled my default.
    - Encrypts s3 objects usingkeys handled,managed and owned by AWS.
  
  - Server-side Encryption with KMS Keys stored in AWS KMS(SSE-KMS)
    - Leverages AWS Key management service (AWS KMS) to manage encryption keys.

  - Server-side Encryption with customer provided Keys (SSE-C)
    - Customers provide thier own keys to use for encryption of objects in s3.

- **Client-Side Encryption**

  - Uae clinet libraries such as Amazon S3 Client-Side Encryption Library.
  - Clients must encrypt data themselves before sending to Amazon S3 likewise they must decrypt data too before retrieving from S3.
  - Customer fully manages the keys and ecryption cycle.

### Encryption in Transit (SSL/TLS)

  Encryption in flight is also called SSL/TLS.

- Amazon S3 exposes 2 endpoints:
  - HTTP endpoint -non encrypted
  - HTTPS endpoint - Encrypted

- HTTPS is Recommended
- HTTPS is mandatory for SSE-C(customer-provided keys).

## What is CORS?

Stands for **Cross Origin Resource Sharing** where;

- Origin = scheme(protocol) + host(domain) + port
  - example: <https://www.example.com> (implied port is 443 for https, 80 for http)

- Cors is a web browser mechanism to allow requests to other origins while visiting the main origin

## MFA Delete

MFA is required to :

- Permanently delete an object version
- suspend versioning on the buckets.

On the Other hand, MFA will not be required to:

- Enable versioning
- List deleted versions

To use MFA Delete, versioning must be ensbled on the bucket

Only the bucket owner(root account) can enable/disable MFA Delete.

## S3 Access logs

- For audit purposes, you may want to log all access to s3 buckets.

- Any request made to any account weather accepted or denied will be logged into the logs bucket for the data to be analysed using data analysis tools.

- The target logging bucket must be in the same AWS region.

## S3 Glacier Vault Lock

- Adopt a WORM(Write Once Read Many) model.
- Create a vault lock policy.
- Lock the policy from future edits(can't be changed or deleted)
- Helpful for compliance and data retention.

## AWS CloudFront And AWS Global Accelerator

### What is AWS Cloudfront?

- It's a content delivery network(CDN).
- **It improves read performance, content is cached at the edge locations.
- Improves user experience.
DDoS protection(because its worldwide),also integrated with shield, AWS WAF.

## AWS Storage Extras

### Aws Snow Family

- Highly secure and portable devices that is used by aws to collect and process data at the egde and for data migration in and out of aws.
- For Data migration, we can use
  - snowcone,
  - snowball Edge,
  - Snownobile

- Edge Computing:
  - snowcone
  - snowballl edge

### Snowball Edge (for data transfers)

- Physical hardware solution that is used for transporting large data. Could be terabytes or petabytes of data in and out of aws.
- Good alternative to moving data over the network which can cost you alot in network fees.
- Pay per data transfer Job.
- We have 2 kinds:
  - **snowball edge storage optimized**(80 TB HDD or 210 TB NVMe for block volume and s3 compatible object storage)
  - **Snowball edge Compute Optimized**(42 TB HDD r 28 TB NVMe capacity for block volume and s3 compatible object storage)
- Use cases will be ;
  - Data cloud migrations
  - Data Center Decommissioning
  - Disaster Recovery

### AWS Snowcone & Snowcone SSD

- This device is small and portable, can withstand harsh environments and also very secure.
- Light(about 4.5lbs or 2.1kg)
- Device can be used for edge computing, storage and data transfer.
- This device comes in 2 flavors
  - **Snowcone** : 8TB of HDD storage
  - **Snowcone SSD** : 14TB pf SSD Storage
- Can be used where snowball does not fit (confined space) but you need to provide your own batteries.
- Can be sent back to AWS offline or connect to internet and use AWS DataSync to send data.

## AWS SnowMobile

- It's an actual truck, very secured, comes with 24/7 surveillance.
- Transfer exabytes of data (1exabyte = 1000 petabyte = 1,000,000 Terabytes)
- Each snowmobile has 100 PB of capacity (use multiple in parallel).

![aws snow family for data migration](/EC2-Fundamentals/Snowball.png)

### Amazon FSx - Overview

- This is a 3rd party high-performance file systems on AWS
- Fully managed service
- Examples include;
  - FSx for Lustre
  - FSx for Windows File Server
  - FSx for NetApp ONTAP
  - FSx for OpenZFS

## Hybrid Cloud for Storage

- AWS is starting to push for "hybrid cloud".
- Reasons for this can be
  - Long cloud migration time
  - Security Requirements
  - Compliance requirements
  - IT strategy
- S3 is a proprietary storage technology (unlike EFS/NFS), so how do you expose the S3 data on-prem? **AWS Storage Gateway**

### AWS Storage Cloud Native Options

- **BLOCK**
  - Amazon EBS & EC2 instance store

- **FILE**
  - Amazon EFS & Amazon FSx

- **OBJECT**
  - Amazon S3 & Amazon Glacier

## AWS Storage Gateway

- It's a bridge between on-prem data and cloud data
- Use cases:
  - disaster recovery
  - backup and restore
  - tiered storage
  - on-prem cache and low latency files access

- Types of storage gateway
  - S3 file gateway
  - FSx file gateway
  - Volume gateway
  - Tape gateway

### AWS Transfer Family

- A fully-managed service for file transfers into and out of Amazon S3 or amazon EFS using FTP protocol
- Supported protocols include
  - AWS Transfer for FTP ( File Transfer Protocol)
  - AWS Transfer for FTPS ( File Transfer protocol over SSL (FTPS))
  - AWS Transfer for SFTP ( Secure File Transfer Protocol (SFTP))
- Managed infrasture, scalable, Reliable, Highly available (multi-AZ)
- Pay per provisioned endpoint per hr plus data transfers in GB
- Store and manage users credentials within the service.
- Can integrate with existing authentication systems (MicroSoft Active Directory, LDAP, OKta, Amazon Cognito, Custom)
- Usage: Sharing Files, public datasets, CRM, ERP

## AWS DataSync

- Move large amount of data to and from
  - on-prem / other cloud to AWS (NFS, SMB, HDFS, S3 API) and needs an agent to run on-prem or the other cloud.
  - AWS to AWS (different storage services) requires no agent.
- Can synchronize to:
  - Amazon S3 (any storage classes - including glacier)
  - Amazon EFS
  - Amazon FSx (windows, lustre, NetApp, OpenZFS)
- Replication task can be scheduled hourly,daily,weekly
- Has the ability to keep file permissions and metadata are preserved (NFS POSIX,SMB)
- One agent task can use 10Gbps and you can setup a bandwidth limit.

![storage-compare](/EC2-Fundamentals/storage-compare.png)

## Decoupling Applications: SQS,SNS,Kinesis, Active MQ

### Introduction

- Forms of communications that we will look at is

**1. Synchronous Communications(Application to Application**
![synchronous-communication](/EC2-Fundamentals/asynch.png)

**2. Asynchronous/Event based Communications(Application to queue Application**
![asynchronous-communication](/EC2-Fundamentals/asyn2.png)

- We could face scaling challenges if there is a spike in traffic, in the synchronous model. So in this case, it's better to decouple your application to avoid such. We can use the Asynchronous model using:
  - SQS: queue model
  - SNS: Publisher/subscriber model(pub/sub)
  - Kinesis: Real-time streaming model
- The services above are good because they can scale independently from our application.

### Amazon SQS

**What is a Queue?**

- Producers send messages to the queue and the consumers poll messages from the queue and deletes the message from the queue.

![sqs-queue](/EC2-Fundamentals/sqs-queue.png)

- The oldest AWS offering
- Fully managed service, used to decouple applications

Some of the things to take note of are

- Unlimited throughput,unlimited number of messages in queue
- Default retention of messages is 4days & max of 14days
- Very low latency and has a limit of 256kB per message sent.
- Very low latency (< 10ms on publish and recieve )
- Messages can be delivered twice or duplicate messages(at least once on delivery)
- Can have out of order messages(best effort offering)

### SQS -Producer (Producing Messages)

- Messages are produced by producers to SQS using the SDK(Software Developement Kit) through an API called *"SendMessage API"*
- The message is then persisted in SQS until a *consumer* reads, processes and deletes  the message.

**Use Case**

Send an order to be processed

- Order id
- Customer id
- Any attributes you want

*- SQS standard has unlimited throughput.*

### SQS -Consumers (Consuming Messages)

- Consumers are applications that you write with code,
- These consumers can be running on EC2 Instances, Servers, or AWS Lambda
- Consumers can poll up to 10 messages from SQS at a time.
- Once the consumer processes the message, the message is deleted(Unsing the *DeleteMessage API*) so that no other consumer is able to see the message and the process is complete.

**Use Case**

Insert the messages pulled from SQS into a database.

![sqs-consumer](/EC2-Fundamentals/sqs-consumer.png)

### SQS - Multiple EC2 Instaces As Consumers

- In this case, consumers receive and process messages in parallel to one another using a poll function.
- If once consumer delays in receiving a message, another consumer will receive it.
- We can scale consumers horizontally to improve throughput of processing.
- This also means that we are able to integrate SQS with our Auto Scaling Group(ASG).
  - Consumers will be running on instances inside an ASG.
  - Your ASG has to be scaling on some kind of metric. One of the metric we can configure is called *queue length*. Its a Cloudwatch Metric(approximate number of messages).
- We can setup a cloudwatch alarm when a threshold is met, so that the alarm can trigger the ASG to scale our consumers(instances) up or down.

![SQS-ASG](/EC2-Fundamentals/asg-sqs.png)

### SQS - Security

- Encryption
  - We have in-flight ecryption using HTTPS API
  - At-rest encryption using KMS keys
  - Client-side encryption if the client wants to perform encryption/decryption itself but its not provide by AWS out-of-the-box.  

- Access Controls:
  - IAM Policies are available to regulate access to the SQS API

- SQS Access Policies(Similare to S3 bucket policies)
  - Useful for cross-account access to SQS queues
  - Useful for allowing other services (SNS,S3) to write to an SQS queue.

### SQS Visibilty Timeout

- After a message is polled by the consumer, it becomes invisible to other consumers after a particular time(default is 30seconds.)
- If the messages are not processed during the visibily time-out, it will be processed twice.
- Consumer can call the *ChangeMessageVisibilty* API to get more time.
if visibilty timeout is high(hrs) and consumer crashes,reprocessing will take time.
- If visibilty is too low(seconds) we may get duplicates

![message-visibilty](/EC2-Fundamentals/msg-vis.png)


### Amazon SNS(Simple Notification Service)

- Good use case will be if you want to send one message to many receivers. Example will be a buying service wanting to send notification to multiple services.
![sns-topic](/EC2-Fundamentals/pub-sub-sns.png)

- The "event producer" sends only one message to one SNS topic
- We can have as many "event receivers"(subscriptions) as we want to listen to the SNS topic notifications
- Each subscriber to the topic will get all the messages (there's a new feature to filter messages)
- Can have up to 12.5million subscriptions per topic.
- 100,000 SNS topic limit.

![SNS-2](/EC2-Fundamentals/sns-2.png)

**- One important point to note is that many aws services can send data directly to SNS for notifications.

### SNS + SQS: Fan Out

- Push once in SNS, receive in all SQS queues that are subscribers.
- Fully decoupled, no data loss
- SQS allows for data persistence, delayed processing and retries of work.
- Ability to add more SQS subscribers over time.
- Make sure your SQS queue access policy allows for SNS to write
- Cross-Region delivery if the security rules allows it, hence can work with SNS queues in other regions.

![fan-out](/EC2-Fundamentals/fan-out.png)

### SNS Message Filtering

- It's a  JSON policy used to filter messages sent to SNS topics's subsriptions.
- If a subscription doesn't have a filter policy, it receives every message.

![filtering-messages](/EC2-Fundamentals/filtering.png)

### Amazon Kinesis

- Makes it easy to collect, process and analyze streaming data in real time.
- This could be application logs, metrics, website clickstreams, IoT telemetary data.

- we have about 4 services under kinesis
  - **Kinesis Data Streams** to capture, process and store data streams
  - **Kinesis Data Firehose** to load data streams into AWS data stores.
  - **Kinesis Data Analytics** to analyze data streams with SQL or Apache Flink
  - **kinesis Video Streams** to capture, process and store video streams.

### i. Kinesis Data Streams

  Uses a method called sharding(a process of storing large database across multiple machines).

![kinesis-data-stream](/EC2-Fundamentals/kds.png)

- Retention of data by producers is between 1 to 365days
- Has the ability to replay data
- Once data is introduced to kinesis, it cannot be deleted(immutabilty)
- Data that have the same partition keys goes to the same shard
( [For more info on Database Sharding - AWS](https://aws.amazon.com/what-is/database-sharding/)
 )
- Data that shares same partition keys goes to the same shard.
- **Producers** i.e
  - AWS SDK, Kinesis Producer Library (KPL), kinesis agent.
- For **Consumers** i.e
  - you can write your own Kinesis client Library(KCL), AWS SDK
  - Managed: AWS Lambda, Kinesis Data Firehose, Kinesis Data Analytics.

### Capacity Mode in Kinesis Data Streams

- **Provisioned Mode:**
  - You can choose the number of shards you want provisioned, scaled manually or using API.
  - Each shard gets 1MB/s in (or 1000 records per second)
  - Each shard can get 2MB/s out (classic or enhanced fan-out consumer)
  - You pay per shard provisioned per hour.

- **On-Demand Mode:**
  - No need for you to provision or manage the capacity
  - Default capacity provisioned (4 MB/s in or 4000 records per second)
  - Scales automatically based on observed throughput peak during the last 30days.
  - You pay per stream per hour and data in/out per GB.

### Kinesis Data Streams Security

- Can control access using IAM policies
- Ecryption of data in flight using HTTPS and at rest using KMS,
- Can impliment encryption/decryption of data on client side but it's difficult.
- VPC endpoints available to kinesis to access within VPC.
- Monitor API calls using cloudtrail.

### Kinesis Data Firehose

Amazon Data Firehose provides the easiest way to acquire, transform, and deliver data streams within seconds to data lakes, data warehouses, and analytics services

![kinesis-firehost](/EC2-Fundamentals/firehost.png)

- Amazon Data Firehose is a fully managed service for delivering real-time streaming data( [read more](https://aws.amazon.com/what-is/streaming-data/) ) to destinations such as 
  - Amazon Simple Storage Service (Amazon S3), Amazon Redshift, Amazon OpenSearch Service, Amazon OpenSearch Serverless,
  - Splunk, and any 
  - custom HTTP endpoint or HTTP endpoints owned by supported third-party service providers, including Datadog, Dynatrace, LogicMonitor, MongoDB, New Relic, Coralogix, and Elastic.
- With Amazon Data Firehose, you don't need to write applications or manage resources. You configure your data producers to send data to Amazon Data Firehose, and it automatically delivers the data to the destination that you specified.
- You can also configure Amazon Data Firehose to transform your data before delivering it.

### Differences between Kinesis Data Streams and Firehose

![Data stream vs firehose](/EC2-Fundamentals/ds-firehorse.png)


### Ordering data into Kinesis

**Scenario**

- You have multiple trucks(about 5 trucks) on the road sending their ETA to AWS regularly.
- You want to consume the data from each truck in an orderly manner so that you can track their movement. How should you send the data into Kinesis?

**Answer**

- Use *"Partition key"* with the value being the *"truck id"* because the same key will always go to the same shard.

![order data in kinesis](/EC2-Fundamentals/order-kinesis.png)

### Ordering data into SQS

- We have no data ordering in SQS.
- For SQS FIFO, if you don't use a *group ID*, messages are consumed in a first-come first-serve bases with only one consumer.

### Amazon MQ

- Applications on-prem may use traditional protocols such as MQTT, AMQP, STOMP, Openwire, WSS. 
- Companies may want to use the same protocols when migrating to the cloud rather than use the SNS and SQS APIs. AWS makes this possible by offering Amazon MQ,
- Amazon MQ is a managed message broker service for RabbitMQ and ActiveMQ.
- It doesn't scale like SNS and SQS.
- It runs on servers and can run in multi-AZ with failover.
- It cames both with queue feature like SQS and topic features like SNS.

### High Availabilty in Amazon MQ.

![ha-Amazon MQ](/EC2-Fundamentals/ha-MQ.png)

## Containers on AWS(ECS, Fargate, ECR & EKS)

### Amazon ECS(Elastic Container Service)

  Amazon Elastic Container Service (Amazon ECS) is a fully managed container orchestration service that helps you easily deploy, manage, and scale containerized applications.
  
- As a fully managed service, Amazon ECS comes with AWS configuration and operational best practices built-in.
- It's integrated with both AWS and third-party tools, such as Amazon Elastic Container Registry and Docker. This integration makes it easier for teams to focus on building the applications, not the environment.
- You can run and scale your container workloads across AWS Regions in the cloud, and on-premises, without the complexity of managing a control plane.

### Launch Type - EC2 Instances

- Launch Docker Containers on AWS = Launch ECS Tasks  on ECS Clusters
- EC2 Launch Type; you must provision and maintain the infra.(ec2 instances which should all have ECS agents running on it, so that it can be registered to the ECS Cluster.)
- AWS takes care of starting and stopping the containers. 

![launch-type](/EC2-Fundamentals/launch-type.png)

### Launch Type - Fargate(serverless)

- Launch docker containers on AWS.
- It's managed by AWS, you do not need to manage the underlying infrastructure(serverless)
- You create the task definitions and AWS runs the task for you based on the CPU/RAM needed.
- You can scale quickly by increasing the number of task.

### IAM Roles For ECS

- **For EC2 Instance Profile(EC2 Launch Type Only)**:
  - Used by the ECS agent only to make api calls to the ECS service.
  - Sends container logs  to cloudwatch logs.
  - Pulls docker images from ECR and references sensitive data in Secrets Manager or SSM Parameter store.

- **For ECS Task Role**:
  - Allows each task to have a specific role and 
  - Uses different roles for the different ECS services you run
  - Task role is defined in the task definition. 

![ecs-launch-type](/EC2-Fundamentals/ecs-2.png)

### ECS Service Auto Scaling

- Can automatically increase/decrease the desired number of ECS tasks
- ECS auto scaling uses **AWS application Auto Scaling**. Scaling can be done in 3ways
  - ecs service average CPU utilization
  - ecs service average Memory utilization - Scale on RAM
  - ALP request count per target -metric comes from alb
- Target Tracking: will scale based on the value for a specific cloudwatch metric.
- Step Scaling based on the specified cloudwatch alarm
- Scheduled Scaling based on a specified date/time (predictable changes)

### Amazon ECR(Elastic Container Registry)

- Stores and manage docker images on AWS
- Has both private and public cloud repos(Amazon ECR Public Galery)
- Fully integrated with ECS and backed by S3
- Access control through IAM Role
- Supports imaged vulnerability scanning, versioning, image tags, image lifecycle.

### Amazon EKS(Elastic Kubernetes System)

- Similar to ECS but different API
- Supports 2 launch modes: EC2 launch modes line instances or wireless launch modes like Fargate to deploy to serverless containers.

### Amazon EKS Node Types

- **Managed Node Groups**
  - Creates and manages the node(EC2 instances) for you.
  - Part of an ASG managed by EKS
  - Supports On-Demand and Spot Instances

- **Self-Managed Nodes**
  - Nodes are created you and registered to the EKS cluster and managed by an ASG
  - You can use pre build AMI Amazon EKS Optimized AMI
  - Supports On-Demand or spot instances

- **AWS Fargate**
  - No maintainance required because it's a wireless solution, no nodes are being managed.

### Data Volumes on EKS

- You need to specify *StorageClass* to your EKS cluster
- Leverages a container Storage interface(CSI) compliant driver
- Can support:
  Amazon EBS
  Amazon EFS(works with Fargate)
  Amazon FSx for Lustre
  Amazon FSx for NetApp ONTAP 

### AWS App Runner

- Fully managed web service that makes it very easy to deploy web apps and APIs at scale.
- Don't need any infrastructure experience.
- Start with your source code or container image
- Automatically build and deploy the web app
- Automatic scaling, highly available, load balancing, ecryption.
- VPC access support
- Connect to database, cache, and message queue services
- Use cases: web apps, APIs microservices, rapid production deployments.

## AWS Lambda

### Why Lambda?

- Are virtual functions -no servers to manage.
- Limited by time; shorter executions (15mins)
- Runs on demand, The only run when the have been invoked.
- Scaling is automated.

### Benefits of Lambda

- The pricing is easy:
  - Pay per request and compute time
  - Free tier of 1million AWS lambda requests and 400,000GBs of compute time.
- Integrated with the whole suite of services
- Integrated with many programing languages
- Easy monitoring through AWS CloudWatch
- Easy to get more resources per functions (up to 10GB of RAM).
 Increasing the RAM, will also improve CPU and network.

### AWS Lambda Language support

- Node.js(JavaScript)
- Python
- Java (java8 compatible)
- C# (.Net Core)
- Golang
- C# / Powershell
- Ruby
- Custom Runtime API (community supported, example Rust)
- Lambda container image
  - The container image must impliment the lambda Runtime API
  - ECS / Fargate is preferred for running arbitrary Docker images

### Lambda SnapStart

- AWS Lambda SnapStart is a feature designed to improve the startup performance of Lambda functions, particularly for those with long initialization times. SnapStart achieves this by creating a snapshot of a Lambda function's execution environment after it has been initialized, which can then be used to quickly start new instances of the function.