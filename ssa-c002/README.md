# AWS SAA-C02 Study Guide

### 1. Identity Access Management (IAM)

- IAM is a **Global Service**. 
- Root account is created when you setup AWS account and has complete ADMIN access.
- New USER have now permission when created. In AWS you apply _least privilage principle_ i.e. dont give user more permission than the user needs.
- 3 options to access AWS 
  1. AWS Management Console (protected by password + MFA)
  2. AWS Command Line Interface (CLI): protected by access keys
  3. AWS Software Developer Kit (SDK) - for code: protected by access keys


**IAM Entities**
- Users: End users such as people, employees of an Organisation etc. User can belong to multiple group. As well as they dont have to belong to a group
- Groups: Contains only a collection of users. Each user in the group will inherit the permissions of the group. Group **does not contain** other GROUPS 
- Policies: Policies are made up of documents, called Policy documents. These documents are in format called JSON and they give permissions as to what a User/Group/Role to do.
- Roles: You create a role and them Assign them to AWS Resources.

**Priority Levels in IAM:**
- **Explicit Deny:** Denies access to a particular resource and this ruling cannot be overruled.
- **Explicit Allow:** Allows access to a particular resource so long as there is not an associated Explicit Deny.
- **Default Deny (or Implicit Deny):** IAM identities start off with no resource access. Access instead must be granted.

**IAM Policies Structure**
- ###### Consists of 
  - **Version**: policy language version, always include “2012-10-17”
  - **Id**: an identifier for the policy (optional) 
  - **Statement**: One or more individual statements (required)
- ###### Statements consists of 
  - **Sid**: an identifier for the statement (optional) 
  - **Effect**: whether the statement allows or denies access(Allow, Deny)
  - **Principal**: account/user/role to which this policy applied to 
  - **Action**: list of actions this policy allows or denies 
  - **Resource**: list of resources to which the actions applied to 
  - **Condition**: conditions for when this policy is in effect(optional)

**IAM Security Tools**
- **IAM Credentials Report (account-level)**
  - A report that lists all your account's users and the status of their various credentials
 
- **IAM Access Advisor (user-level)**
  - Access advisor shows the service permissions granted to a user and when those services were last accessed.
  - You can use this information to revise your policies.

**IAM Guidelines & Best Practices**
- Don’t use the root account except for AWS account setup
- One physical user = One AWS user
- Assign users to groups and assign permissions to groups
- Create a strong password policy
- Use and enforce the use of Multi Factor Authentication (MFA)
- Create and use Roles for giving permissions to AWS services
- Use Access Keys for Programmatic Access (CLI / SDK)
- Audit permissions of your account with the IAM Credentials Report
- Never share IAM users & Access Keys

### 2. Elastic Cloud Compute (EC2)

**EC2 Overview**
- **IAAS** - EC2 instance are virtual machines => Instances (unit of consumption is instance)
- Private service by-default - uses VPC networking
- **AZ resilient** - Instance fails if AZ fails
- On-Demand Billing - Per second
- EC2 instances run on EC2 hosts. These host are either **Dedicated** or **Shared**.
- **Share hosts** are host which are shared across different AWS customers, so _you don’t get any ownership of the hardware and you pay for the individual instances._ Every _customer using shared host are isolated from each other._ 
  - Shared host are the **Default**
- **Dedicated host** you are paying for the entire host not just the instance it runs on.
- EC2 is an **AZ resilient service**. The reason for this is that the host themselves run inside a single AZ. If the AZ fails, the host inside that AZ could fail and any instance running on any host that fails will fail. As an SA you have to assume if AZ fails than instances running inside that AZ will fail.
- EC2 have some local hardware .I.e logically CPU, Memory and some local storage also called Instance Store.
- **Instance store is temporary**
- They have 2 types of Networking , Storage networking and data networking.
- EC2 can connect to a remote storage .i.e. ELASTIC BLOCK STORAGE (EBS)
- Instance running on a specific host will stay on that host when restarted. Instance stay on the host until one of the 2 things happen
  1. The host fails or it is taken down for maintenance.
  2. Secondly if the instance is stopped and then restarted. This is different to just restarting
***EC2 and EBS are both AZ service. You can never connect Network Interfaces or EBS storage located in one AZ to an EC2 instance in another AZ.

**EC2 sizing & configuration options**
- Operating System (OS): Linux, Windows or Mac OS 
- How much compute power & cores (CPU) 
- How much random-access memory (RAM) 
- How much storage space: 
  - Network-attached (EBS & EFS) 
  - Hardware (EC2 Instance Store) 
- Network card: speed of the card, Public IP address 
- Firewall rules: security group 
- Bootstrap script (configure at first launch): EC2 User Data

**EC2 Instance type** s (https://aws.amazon.com/ec2/instance-types/)
- **General Purpose**: Default - Diverse workloads, equal resource ratio
- **Compute Optimised** : This is used for Media processing, HPC, Scientific Modeling, gaming, Machine learning. They provide access to the latest high performance CPU’s and they offer a ratio where more CPU is offered than Memory.
- **Memory Optimised** : It is logically the inverse of Compute Optimised. It offers large memory allocation. This is ideal for applications processing large in-memory datasets, caching and database workloads.
- **Accelerated Computing** : It is where additional capabilities come into play, such as dedicated GPU’s for high scale parallel processing and modelling 
- **Storage Optimised** : It provides large amounts of super fast local storage, either designed for high sequential transfer rates or provide massive amounts of IO operations per second.

**EC2 Instance Purchase option and pricing**
- **EC2 - On Demand**
  - Per second billing while an instance is running.  So you are paying for the resources you consume. Associated resources such as storage, are charged regardless of instance state. 
  - It is the Default purchase option so you should start your evaluation process. Start with On-demand and then move to other instance type if you can justify.
  - There are no interruptions and you launch and instance and you pay per second charge. Barring any failures the instance runs until you decide otherwise. 
  - You dont receive any capacity reservations with On-Demand.
  - If AWS has a major failure and capacity is limited, Reserved purchase options receives highest provisioning priority on whatever capacity remains.
  - Recommended for short-term and un-interrupted workloads, where you can't predict how the application will behave

**EC2 - Reserved Instance**
- Ensure you keep the instance for 1 year (+discount) or 3 years (+++discount), Minimum requirement 1200 hours per year and 1 year term.
- Reserved instances takes priority for AZ capacity. 
- Does not support all instance types or regions
- Great if you have a known **stead state usage**, email, domain ot database server. Cheapest option with no tolerance for disruption.
- Up to 72% discount compared to On-demand
- Payment Options – No Upfront (+), Partial Upfront (++), All Upfront (+++)
- **Scheduled Reserved Instance**
  - Ideal for long term usage which does not run constantly
  - Does not support all instance types or regions
  - Minimum requirement 1200 hours per year and 1 year term.
- **Capacity Reserved instance**
- **Regional reservation** provides a billing discount for valid instances launched in any AZ in that region. While flexible they dont reserve capacity within an AZ. This is risky during major faults, when capacity can be limited.
- **Zonal reservations** only apply to one AZ providing billing discounts and capacity reservation in that AZ.
- You can buy and sell in the Reserved Instance Marketplace
- Convertible Reserved Instance
  - Can change the EC2 instance type, instance family, OS, scope and tenancy
  - Up to 66% discount
- **Note**: Reserved Instances that are terminated are billed until the end of their term.



**EC2 Savings Plans**
- Get discount based on long-term usage (up to 72% - same as Reserved Instance)
- Commit to a certain type of usage ($10/hour for 1 or 3 years) and get houly rate commitment
- Usage beyond EC2 Savings Plans is billed at the On-Demand price
- Locked to a specific instance family & AWS region (e.g., M5 in us-east-1)
- Flexible across:
  - Instance Size (e.g., m5.xlarge, m5.2xlarge)
  - OS (e.g., Linux, Windows)
  - Tenancy (Host, Dedicated, Default)

**EC2 - Spot Instance**
- It is the cheapest way to get access to EC2 
- Never use spot instances for workload that cannot handle interruptions. 
- If a spot instance is terminated by Amazon you will not be charged for partial hour of usage. However if you terminate the instance you will be charged.
- **Spot Block**
  - “block” spot instance during a specified time frame (1 to 6 hours) without interruptions
  - In rare situations, the instance may be reclaimed

**Spot Fleets**
- Spot Fleets = set of Spot Instances + (optional) On-Demand Instances
- The Spot Fleet will try to meet the target capacity with price constraints
  - Define possible launch pools: instance type (m5.large), OS, Availability Zone
  - Can have multiple launch pools, so that the fleet can choose
  - Spot Fleet stops launching instances when reaching capacity or max cost
- Strategies to allocate Spot Instances:
  - lowestPrice: from the pool with the lowest price (cost optimization, short workload)
  - diversified: distributed across all pools (great for availability, long workloads)
  - capacityOptimized: pool with the optimal capacity for the number of instances
- Spot Fleets allow us to automatically request Spot Instances with the lowest price

**EC2 - Dedicated hosts**
- This is an EC2 host which is allocated to you in its entirety. 
- No Charge for the instances running on the host
- Normally used where you have software which is licensed based on sockets or cores in a physical machine.
- Dedicated hosts also have a feature called Host Affinity linking instances to certain EC2 hosts.So if you stop and start an instance it remains on the same host.

**EC2 - Dedicated Instances**
- Instances run on hardware that’s dedicated to you
- May share hardware with other instances in same account
- No control over instance placement(can move hardware after Stop / Start)

**Standard Reserved vs. Convertible Reserved vs. Scheduled Reserved:**
Standard Reserved Instances have inflexible reservations that are discounted at 75% off of On-Demand instances. Standard Reserved Instances cannot be moved between regions. You can choose if a Reserved Instance applies to either a specific Availability Zone, or an Entire Region, but you cannot change the region.
Convertible Reserved Instances are instances that are discounted at 54% off of On-Demand instances, but you can also modify the instance type at any point. For example, you suspect that after a few months your VM might need to change from general purpose to memory optimized, but you aren't sure just yet. So if you think that in the future you might need to change your VM type or upgrade your VMs capacity, choose Convertible Reserved Instances. There is no downgrading instance type with this option though.
Scheduled Reserved Instances are reserved according to a specified timeline that you set. For example, you might use Scheduled Reserved Instances if you run education software that only needs to be available during school hours. This option allows you to better match your needed capacity with a recurring schedule so that you can save money.

**EC2 Placement Groups**
benefits and limitations of the three placement groups available within AWS :
  **Cluster Placement Groups (PERFORMANCE)** 
  - Clusters instances into a low-latency group in a single Availability Zone
  - This is recommended for applications that need the lowest latency possible and require the highest network throughput.
  - Only certain instances can be launched into this group (compute optimized, GPU optimized, storage optimized, and memory optimized).
  - It offers little to no resilience.
**** YOU CANNOT span Availability zones with clusters  placement groups. ONE AZ ONLY This is locked when launching the first instance. You can span VPC peers but this does significantly impact performance in a negative way.****

  **Spread Placement Groups (Resilience)** 
  - Designed to ensure maximum amount of availability and resilience for an application.
  - It can SPAM MULTIPLE AZ
  - There is limit of **7 instances per AZ and per placement group**
  - THEY PROVIDE INFRASTRUCTURE ISOLATION
  - Each Instance runs from a different physical hardware .i.e. different rack and each rack has its own network and power source.

  **Partition Placement Groups (Topology Awareness)** - groups of instances spread apart
  - Partitioned Placement Grouping is similar to Spread placement grouping, but differs because you can have multiple EC2 instances within a single partition.
  - Designed for huge scale parallel processing systems.
  - **7 Partitions per AZ**
  - Can span across multiple AZs in the same region
  - Up to 100s of EC2 instances
  - The instances in a partition do not share racks with the instances in the other partitions
  - A partition failure can affect many EC2 but won’t affect other partitions
  - EC2 instances get access to the partition information as metadata
  - Great for topology aware applications like HDFS, HBASE and Cassandra.
  - Instances can be placed in a specific partition

**Elastic Network Interfaces (ENI)**
- elastic network interface is a logical networking component in a VPC that represents a virtual network card.
- Each instance has a default network interface, called the primary network interface. You cannot detach a primary network interface from an instance.
- ENI is used mainly for low-budget, high-availability network solutions. However, if you suspect you need high network throughput then you can use Enhanced Networking ENI.
- Enhanced Networking ENI uses single root I/O virtualization to provide high-performance networking capabilities on supported instance types. 
- SR-IOV provides **higher I/O** and **lower throughput** and it ensures **higher bandwidth, higher packet per second (PPS) performance**, and consistently **lower inter-instance latencies**. 
- No Charge as it is available on most EC2 types

**EC2 Nitro**
- Underlying Platform for the next generation of EC2 instances
- New virtualization technology
- Allows for better performance:
  - Better networking options (enhanced networking, HPC, IPv6)
  - Higher Speed EBS (Nitro is necessary for 64,000 EBS IOPS – max 32,000 on non-Nitro)
- Better underlying security


### 3. Elastic Block Storage (EBS)
An EBS (Elastic Block Store) Volume is a network drive you can attachto your instances while they run. It allows you to persist data.
- Since it is a Network drive, it uses the network to communicate the instance, which means there might be a bit of latency
- EBS is an Availability zone service. It’s locked to an Availability Zone (AZ). i.e you can attach it to EC2 instance in the same availability zone.
- In order to move a volume across, you first need to create a snapshot of the volume.
- EBS replicates within an AZ. Failure of an AZ means failure of a volume
- An EBS volume can only be attached to one EC2 instance at a time.
- EBS Volumes offer 99.999% SLA.
- By default, the root EBS volume is deleted (Delete on Termination attribute is enabled), any other attached EBS volume is not deleted as this attribute is disabled.
- You can take a backup (snapshot) of your EBS volume at a point in time. it is Not necessary to detach volume to do snapshot, but recommended. You can copy snapshots across AZ or Region
- EBS volume have a provisioned capacity (size in GBs, and IOPS). You get billed for all the provisioned capacity.
- EBS Snapshot has 2 features
  - EBS Snapshot Archive, this allows you to move Snapshot to archive tier that is 75% cheaper however it takes within 24 to 72 hours for restoring the archive
  - Recycle Bin for EBS Snapshots, where you setup rules to retain deleted snapshots. It allows you to recover them after an accidental deletion.  retention (from 1 day to 1 year)

**EC2 Instance Store**
• If you need a high-performance hardware disk, use EC2 Instance Store
• Better I/O performance
• Instance Store volume are sometimes called Ephemeral storage because  EC2 Instance Store lose their storage if they’re stopped (ephemeral)
• Good for buffer / cache / scratch data / temporary content
• Risk of data loss if hardware fails
• Backups and Replication are your responsibility. EBS backed instance can be stopped and you will not loose the data on this instance if stopped.

**EBS Volume Types**
- EBS Volumes come in 6 types
  - gp2 / gp3 (SSD): **General purpose SSD** volume that balances price and performance for a wide variety of workloads
  - **io1 / io2 (SSD):** Highest-performance SSD volume for mission-critical low-latency or high-throughput workloads
  - st1 (HDD): Low cost HDD volume designed for frequently accessed, throughput- intensive workloads
  - sc1 (HDD): Lowest cost HDD volume designed for less frequently accessed workloads
  - EBS Volumes are characterized in Size | Throughput | IOPS (I/O Ops Per Sec)
  - Only gp2/gp3 and io1/io2 can be used as boot volumes

**General Purpose SSD** 1 GiB - 16 TiB
• Cost effective storage, low-latency
• System boot volumes, Virtual desktops, Development and test environments
  • gp3:
    • Baseline of 3,000 IOPS and throughput of 125 MiB/s
    • Can increase IOPS up to 16,000 and throughput up to 1000 MiB/s independently
  • gp2:
    • Small gp2 volumes can burst IOPS to 3,000
    • Size of the volume and IOPS are linked, max IOPS is 16,000
    • 3 IOPS per GB, means at 5,334 GB we are at the max IOPS

**Provisioned IOPS (PIOPS) SSD** 
• Critical business applications with sustained IOPS performance Or applications that need more than 16,000 IOPS
• Great for databases workloads (sensitive to storage performance and consistency)
• io1/io2 (4 GiB - 16 TiB):
  • Max PIOPS: 64,000 for Nitro EC2 instances & 32,000 for other
  • Can increase PIOPS independently from storage size
  • io2 have more durability and more IOPS per GiB (at the same price as io1)
• io2 Block Express (4 GiB – 64 TiB):
  • Sub-millisecond latency
  • Max PIOPS: 256,000 with an IOPS:GiB ratio of 1,000:1
• Provisioned IOPS supports EBS Multi-attach 

**Hard Disk Drives (HDD) 125 GiB to 16 TiB**  
- They Cannot be a boot volume
- Throughput Optimized HDD (st1) 
  - Big Data, Data Warehouses, Log Processing 
  - Max throughput 500 MiB/s and max IOPS 500 
- Cold HDD (sc1): 
  -  For data that is infrequently accessed 
  -  Scenarios where **lowest cost is important**
  -  Max throughput 250 MiB/s – max IOPS 250

**EBS Multi-Attach – io1/io2 family**
• Attach the same EBS volume to multiple EC2 instances in **e same AZ**
• Each instance has full read & write permissions to the volume
• Use case:
  • Achieve higher application availability in clustered Linux applications (ex: Teradata)
  • Applications must manage concurrent write operations
• Must use a file system that’s cluster-aware (not XFS, EX4, etc…)

SSD vs. HDD:
SSD-backed volumes are built for transactional workloads involving frequent read/write operations, where the dominant performance attribute is IOPS.**Rule of thumb:** Will your workload be IOPS heavy? Plan for SSD.
HDD-backed volumes are built for large streaming workloads where throughput (measured in MiB/s) is a better performance measure than IOPS. **Rule of thumb:** Will your workload be throughput heavy? Plan for HDD.

**EBS Encryption**
• When you create an encrypted EBS volume, you get the following:
  • Data at rest is encrypted inside the volume
  • All the data in flight moving between the instance and the volume is encrypted
  • All snapshots are encrypted
  • All volumes created from the snapshot
• Encryption and decryption are handled transparently
• Encryption has a minimal impact on latency
• EBS Encryption leverages keys from KMS (AES-256)
• Copying an unencrypted snapshot allows encryption
• Snapshots of encrypted volumes are encrypted
- Process
  - Create an EBS snapshot of the volume
  - Encrypt the EBS snapshot ( using copy )
  - Create new ebs volume from the snapshot ( the volume will also be encrypted )
  - Now you can attach the encrypted volume to the original instance  

**EFS – Elastic File System**
- Managed NFS (network file system) that can be mounted on many EC2
- EFS works with EC2 instances in multi-AZ
- Highly available, scalable, expensive (3x gp2), pay per use
- Uses NFSv4.1 protocol
- Uses security group to control access to EFS
- Compatible with Linux based AMI (not Windows)
- Encryption at rest using KMS
- POSIX file system (~Linux) that has a standard file API
- File system scales automatically, pay-per-use, no capacity planning!

**Amazon Machine Image (AMI) Overview**
- It is a customization of an EC2 instance i.e. You add your own software, configuration, operating system, monitoring
- It enables Faster boot / configuration time because all your software is pre-packaged
- They are built for a specific region (and AMI can be **copied** across regions)
- An EC2 instances can be launched from:
  - A Public AMI: AWS provided
  - Your own AMI: you make and maintain them yourself
  - An AWS Marketplace AMI: an AMI someone else made (and potentially sells)
- AMI Process (from an EC2 instance)
  - Start an EC2 instance and customize it
  - Stop the instance (for data integrity)
  - Build an AMI – this will also create EBS snapshots
  - Launch instances from other AMIs

**Security Groups**
- Security Groups are the fundamental of network security in AWS. They control how traffic is allowed into or out of our EC2 Instances.
- Security groups only contain rules
- Security groups rules can reference by IP or by security group
- Security groups are acting as a “firewall” on EC2 instances, They regulate: 
  - Access to Ports 
  - Authorised IP ranges – IPv4 and IPv6 
  - Control of inbound network (from other to the instance)
  - Control of outbound network (from the instance to other
- Security Group can be attached to multiple instances
- Security group are Locked down to a region / VPC combination
- Security Group live “outside” the EC2 – if traffic is blocked the EC2 instance won’t see it
- _It’s good to maintain one separate security group for SSH acces_
- If your application is **not accessible (time out), then it’s a security group** issue
- If your application gives a **“connection refused“ error, then it’s an application error** or it’s not launched
- All **inbound** traffic is **blocked by default**
- All **outbound** traffic is authorised by default

**Classic Ports to know**
• 22 = SSH (Secure Shell) - log into a Linux instance
• 21 = FTP (File Transfer Protocol) – upload files into a file share
• 22 = SFTP (Secure File Transfer Protocol) – upload files using SSH
• 80 = HTTP – access unsecured websites
• 443 = HTTPS – access secured websites
• 3389 = RDP (Remote Desktop Protocol) – log into a Windows instance



When using an Application Load Balancer to distribute traffic to your EC2 instances, the IP address you'll receive requests from will be the ALB's private IP addresses. To get the client's IP address, ALB adds an additional header called "X-Forwarded-For" contains the client's IP address.
