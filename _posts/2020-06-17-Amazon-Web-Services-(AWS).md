This study guide is a collection of my notes in preparation for the AWS Certified Developer – Associate certification.

# AWS Fundamentals (IAM + EC2)

## AWS Regions

- AWS has **Regions** (cluster of data centers) all around the world
- Example region names: `us-east-1`, `us-east-2`, `eu-west-3`, etc...
- Most AWS services are region-scoped (e.g., EC2). In other words, if you you use a service in region "a" (e.g., `us-east-1`) your data will not be replicated or synchronized in region "b" (e.g., `us-east-2`), so you would have to re-create your infrastructure
- Some AWS services are global services. This means they are **not** linked to any particular region (e.g., Identity and Access Management or IAM)

## AWS Availability Zones (AZs)

- Each region has many availability zones (usually 3 per region, min is 2, max is 6)
- For example AWS Region `ap-southeast-2` (i.e., Sydney) has 3 availability zones: `ap-southeast-2a`, `ap-southeast-2b`, and `ap-southeast-2c`
- Each AZ consists of one or more discrete data centers with redundant power, networking, and connectivity
- AZs are geographically separated from each other so that they are isolated from disasters
- AZs are connected to each other with high bandwidth, ultra-low latency networking

**Note:** regions end with a number and AZs end with a letter

## IAM Introduction

API calls are the heart of the AWS Cloud, as every interaction with AWS is an API call to some service.

Because security is a top priority for all applications, making calls to AWS requires API keys. We can create our own API keys by using AWS Identity and Access Management (IAM).

- IAM (Identity and Access Management)
- IAM enables creation and management of the following AWS security principles:
  - Users
  - Groups
  - Roles
- Root account should only be used once (when account is created for the first time). Otherwise, it should never be used and shared since it has access to everything
- Users must be created with proper permissions
- IAM is the center of AWS
- Policies are written in JSON

At a high level, IAM has:
- Users: usually a person (e.g., a developer) that needs access to certain AWS services. This person will get an account in IAM (which is **not** the root account)
- Groups: contains users that are grouped together. For example, groups can be established by function (e.g., admin, devops, etc..) and teams (engineering, design, etc..). This allows permissions to be applied to groups and for users to inherit these permissions when they are added to a group
- Roles: for internal usage within the AWS resources and services. Roles is what is given to machines to define access
- Policies: JSON documents which define what Users, Groups, and Roles can and cannot do

**Important distinction:** Users is for physical people and Roles are for machines

- IAM had a global view. In other words, when you create a User, Group, or Role in your AWS account it will be accross all regions
- Permissions are governed by Policies (JSON)
- MFA (Multi Factor Authentication) can be setup
- IAM has predefined "managed policies" (these are common policies that Amazon has already put together so we don't have to re-write them)
- It's best to give users the minimal amount of permissions they need to perform their job (least privilege principles)
- Big enterprises usually integrate their own repository of users (e.g., Microsoft Active Directory) with IAM. Doing this allows users to login to AWS using their company credentials
- Identity Federation uses the SAML standard (e.g., Microsoft Active Directory is one of the big users of the SAML standard)

IAM takeaway:
- One IAM User per physical person
- One IAM Role per application
- IAM credentials should never be shared
- **Never** write IAM credentials in code
- Never use the root account except for initial setup
- Security is important! AWS can cost a lot of money if you or someone else misuses it
