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
  -
