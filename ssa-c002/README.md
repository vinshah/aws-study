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
