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