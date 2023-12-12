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