# AWS SAA-C02 Study Guide

### 1. Identity Access Management (IAM)

- IAM is a **Global Service**. 
- Root account is created when you setup AWS account and has completed ADMIN access.
- New USER have now permission when created. In AWS you apply least privilage principle i.e. dont give user more permission than the user needs.
- You have 3 options to access AWS 
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
