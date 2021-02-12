This study guide is a collection of my notes in preparation for the AWS Certified Developer – Associate certification.

# AWS Fundamentals (IAM + EC2)

## AWS Regions

- AWS has **Regions** (cluster of data centers) all around the world
- Example region names: `us-east-1`, `us-east-2`, `eu-west-3`, etc...
- Most AWS services are **region-scoped** (e.g., EC2). In other words, if, for example, you you use a service in region `us-east-1` (N. Virginia) your data will not be replicated or synchronized to other regions (e.g., `us-east-2`, etc..), so you would have to re-create your infrastructure
- Some AWS services are **global services**. This means they are **not** linked to any particular region (e.g., Identity and Access Management or IAM)

## AWS Availability Zones (AZs)

- Each region has many availability zones (usually `3` per region, min is `2`, max is `6`)
- For example, AWS Region `ap-southeast-2` (i.e., Sydney region) has 3 availability zones: `ap-southeast-2a`, `ap-southeast-2b`, and `ap-southeast-2c`
- Each AZ consists of one or more discrete data centers with redundant power, networking, and connectivity
- AZs are geographically separated from each other so that they are isolated from disasters
- AZs are connected to each other with high bandwidth, ultra-low latency networking

**Note:** **regions end with a number** and **AZs end with a letter**

## Identity and Access Management (IAM) Introduction

API calls are the heart of the AWS Cloud, as every interaction with AWS is an API call to some service.

Because security is a top priority for all applications, making calls to AWS requires API keys. We can create our own API keys by using AWS Identity and Access Management (IAM).

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
- Policies: JSON documents which **define what Users, Groups, and Roles can and cannot do**

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

## Amazon Elastic Compute Cloud (EC2) Introduction

- EC2 is one of the most popular AWS offerings
- Using EC2 users can:
  - Rent virtual machines (EC2)
  - Store data on virtual drives (EBS)
  - Distribute load across machines (ELB)
  - Scaling the services using an auto-scaling group (ASG)

### Creating an EC2 instance

1. Select an instance type

The EC2 Instance Type determines the *hardware* of the host computer used for your instance. Each instance type offers different compute and memory capabilities

2. Select an Amazon Machine Image (AMI) for your instance

The AMI is a template that contains a software configuration (for example, an operating system, an application server, and applications). From an AMI, you launch an instance, which is a *copy* of the AMI running as a virtual server in the cloud

3. Add Storage

The **root device** for your instance **contains the image used to boot the instance**. The root device is either an Amazon Elastic Block Store (Amazon EBS) volume or an instance store volume.

Your instance may include **local storage volumes**, known as **instance store volumes**, which you can configure at launch time with **block device mapping**. After these volumes have been added to and mapped on your instance, they are available for you to mount and use. If your instance fails, or if your instance is stopped or terminated, the data on these volumes is lost; therefore, these volumes are best used for temporary (ephemeral) data. To keep important data safe, you should use a replication strategy across multiple instances, or store your persistent data in Amazon S3 or Amazon EBS volumes.

4. Add Tags (Optional)

Tags can be used to describe or group instances

5. Configure Security Group

Use Security Groups to restrict access by only allowing trusted hosts or networks to access ports on your instance. For example, you can restrict SSH access by restricting incoming traffic on port 22.

To access your EC2 instance via SSH, you will need a key pair. The key is generated once during instance creation and **cannot be recovered**.

Before SSHing into your instance you will need to set appropriate permissions on the key using:

`chmod 400 EC2Tutorial.pem`

Where "EC2Tutorial.pem" would be replaced by the name of your key file.

As a side note, `chmod 400` sets permissions so that, (U)ser / owner can read, can't write and can't execute. (G)roup can't read, can't write and can't execute. (O)thers can't read, can't write and can't execute ([reference](https://www.quora.com/What-does-chmod-400-mean)).

If you skip this step you won't be able to SSH into your instance and will get the error:

{% highlight c linenos %}
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'EC2tutorial.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "EC2tutorial.pem": bad permissions
ec2-user@54.172.238.16: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
{% endhighlight %}

And, finally, to SSH: `ssh -i EC2tutorial.pem <hostname>@<your-instance-public-ip-address>`

For example: `ssh -i EC2tutorial.pem ec2-user@54.132.218.27`

### EC2 Security Best Practices

- Use AWS Identity and Access Management (IAM) to control access to your AWS resources, including your instances. You can create IAM users and groups under your AWS account, assign security credentials to each, and control the access that each has to resources and services in AWS
- Restrict access by only allowing trusted hosts or networks to access ports on your instance. For example, you can restrict SSH access by restricting incoming traffic on port 22. For more information, see Amazon EC2 security groups for Linux instances.
- Review the rules in your security groups regularly, and ensure that you apply the principle of **least privilege—only** open up permissions that you require. You can also create different security groups to deal with instances that have different security requirements. Consider creating a bastion security group that allows external logins, and keep the remainder of your instances in a group that does not allow external logins.
- Disable password-based logins for instances launched from your AMI. Passwords can be found or cracked, and are a security risk. For more information, see Disable password-based remote logins for root. For more information about sharing AMIs safely, see Shared AMIs.

### Introduction to Security Groups

Security Groups are fundamental to network security in AWS. In essencse, they act as a "firewall" and, thus, **control what traffic is allowed in or out of our EC2 machines**. They regulate:
- Access to ports
- Authorized IP ranges (IPv4 and IPv6)
- Control on ingress network traffic (from other to instance)
- Control of egress network traffic (from instance to other)

![](https://s3.amazonaws.com/gutucristian.com/SecurityGroup.png)

Security Groups Good to Know:
- Can be attached to multiple instances
- Locked down to a region / VPC combination (so if you switch to another region or create another VPC you have to re-create the security groups)
- **Lives outside the EC2 instance**, it is not an appliction running on your instance, so, if traffic is blocked, then EC2 instance won't see it
- **It is good to maintain one security group for SSH access**
- **If **application is not accessible** (time out), then it is a **security group issue**
- **If application gives a "connection refused" error**, then it's an **application error or it hasn't been launched**
- By **default, all inbound traffic is blocked, and all outbound traffic is authorized**

![](https://s3.amazonaws.com/gutucristian.com/SecurityGroup1.png)

We can also reference other security groups within a security group to define allowed inbound and outbound network traffic. 

For example, in the figure below EC2 instance on left has Security Group 1 attached for defining inbound network. This group allows inbound network from group 1 and 2 but not 3. This makes management easier as we no longer have to think about IPs:

![](https://s3.amazonaws.com/gutucristian.com/SecurityGroup2.png)
