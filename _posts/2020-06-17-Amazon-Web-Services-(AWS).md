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
- **If application is not accessible** (time out), then it is a **security group issue**
- **If application gives a "connection refused" error**, then it's an **application error or it hasn't been launched**
- Operate at instance level
- Allow or deny traffic that a NACL allows in
- Are implicit deny -- you can only add "allow" rules
- By **default, all inbound traffic is denied, and all outbound traffic is allowed**
- **Are stateful -- response traffic is automatically allowed**
  - For example, if inbound allows TCP port `80` (HTTP) but denies all outbound traffic and we get a request from the outside to port `80`, a response will still be sent (even though Security Group says all outbound traffic is denied). This is because, by default, all response traffic is automatically allowed. However, we would not be able to initiate a request to the outside world from within the instance since per the Security Group all outbound traffic is denied ([reference](https://www.fugue.co/blog/cloud-network-security-101-aws-security-groups-vs-nacls))

![](https://s3.amazonaws.com/gutucristian.com/SecurityGroup1.png)

We can also reference other security groups within a security group to define allowed inbound and outbound network traffic. 

For example, in the figure below EC2 instance on left has Security Group 1 attached for defining inbound network. This group allows inbound network from group 1 and 2 but not 3. This makes management easier as we no longer have to think about IPs:

![](https://s3.amazonaws.com/gutucristian.com/SecurityGroup2.png)

### Private vs Public IP (IPv4)

- Networking has two sorts of IPs: IPv4 and IPv6
- IPv6 is newer and introduces a greater IP range
- Today, IPv4 is still most common
- IPv4 allows for 3.7 billion different addresses in the public space
- IPv4 format: `[0-255].[0-255].[0-255].[0-255]`
- Public IP:
  - The machine can be uniquely identified on the internet (two machines **cannot** have the same **public IP**)
- Private IP:
  - Using the private IP, the machine can only be identified on the private network that its on
  - The IP of the machine must be unique across the private network that its on
  - Machines connect to the WWW using NAT + internet gateway (proxy)
  - Only a specified range of IPs can be used as private IPs
  
![](https://s3.amazonaws.com/gutucristian.com/PublicPrivateIPNetworkGraph.png)

Things to note:
- **When you stop and then start an EC2 instance public IP will change and private IP will remain as is**
- **By default, an EC2 instance comes with a private IP** for the internal AWS network **and a public IP for the WWW**
- When SSHing into our EC2 machines, we can't use the private IP because we are not in the same network

### AWS Elastic IP

- If you need to have a **fixed public IP** for your instance, you need an **Elastic IP**
- An Elastic IP is a public IPv4 IP you own as long as you don't delete it
- You only pay for an Elastic IP when it is not associated with an EC2 instance
- **You can only have `5` Elastic IP in your account (you can ask AWS to increase that)**
- Overall, try to avoid using Elastic IPs -- they often reflect poor architectural decisions. Instead it is recommended to:
  - Use a random public IP and register a DNS name to, or 
  - Use a load balancer and don't use a public IP
- To use the Elastic IP with your EC2 instance, you have to create an association between the instance and the Elastic IP

### Install Apache on EC2

In this example we will install an Apache HTTP Server (`httpd`) on an EC2 instance to host a basic web application.

The Apache HTTP Server, colloquially called Apache, is a free and open-source cross-platform web server software.

`httpd` is the Apache HyperText Transfer Protocol (HTTP) server program. It is designed to be run as a standalone **daemon process**. When used like this it will create a pool of child processes or threads to handle requests.

We will also use The Yellowdog Updater, Modified (a free and open-source command-line package-management utility for computers running the Linux operating system using the RPM Package Manager), also know as `yum`, to update all instance packages (OS: Amazon Linux 2).

Steps:
1. Create an EC2 instace (select Amazon Linux 2 AMI)
2. Configure instance Security Group to allow inbound SSH (TCP port `22`) and HTTP (TCP port `80`) traffic
3. Create a new SSH key or use an existing one (Note: this is required to be able to get terminal access to instance)
4. SSH into instance using `ssh -i <key-file>.pem ec2-user@<instance-public-ip>
5. Run `sudo su`
  - `sudo` is a program for Unix-like computer operating systems that allows users to run programs with the security privileges of another user
  - `su` is used to switch to another user (**s** witch **u** ser)
  - `sudo su` command switches to the super user – or root user – when you execute it with no additional options
6. Run `yum update -y` to update all system packages
7. Run `yum install -y httpd.x86_64` to install `httpd`
8. Run `systemctl start httpd.service` to start the `httpd` service (listening on port `80`)
9. Run `systemctl enable httpd.service` to make sure service is enabled across reboots
10. Run `curl localhost:80` to see the webpage that will be served
11. To access website via browser, visit the EC2 instance using its **public IP**
  - Note: if you restart the EC2 instance, the public IP will change (unless you associate an Elastic IP with the instance -- in which case the public IP will not change)
12. Run `echo "<h1>Hello World from Nuuk! (Internal DNS: <span style="color:red">$(hostname -f))</span></h1>" > /var/www/html/index.html` to change from default `httpd` page

What is `systemctl`?
- `systemctl` is a controlling interface and inspection tool for the widely-adopted init system and service manager `systemd`
- `systemd` is a suite of basic building blocks for a Linux system. It provides a system and service manager that runs as `PID 1` and starts the rest of the system

`systemctl` references:
- https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units
- https://www.linode.com/docs/guides/introduction-to-systemctl/
- https://wiki.archlinux.org/index.php/systemd

### EC2 User Data

- It is possible to **bootstrap** our EC2 instance using a User Data script
- Bootstraping means launching commands when a machine starts
- The script is **only run once** at **instance start**
- EC2 user data is often used to automate boot tasks such as:
  - Installing updates
  - Installing software
  - Downloading common files from the internet
- The EC2 User Data Script runs with the **root user**

User Data script to automate Apache `httpd` installation from the example above:
{% highlight bash linenos %}
  #!/bin/bash
  yum update -y
  yum install -y httpd.x86_64
  systemctl start httpd.service
  systemctl enable httpd.service
  echo "<h1>Hello World from Nuuk! (Internal DNS: <span style="color:red">$(hostname -f)</span>)</h1>" > /var/www/html/index.html
{% endhighlight %}

A note on why you need `#!/bin/bash`:
- It's a convention so the *nix shell knows what kind of interpreter to run.
- The shebang `#!`, when it is the first two bytes of an executable (`x` mode) file, is interpreted by the `execve(2)` system call (which executes programs). As such, `#!` must be followed by a file path of an interpreter executable. Thus: we have `#!/bin/bash`

### EC2 Instance Launch Types

- **On Demand Instances**: short workload, predictable pricing
- **Reserved**: (MINIMUM one year)
  - Reserved Instances: long workloads
  - Convertible Reserved Instances: long workloads with flexible instances
  - Scheduled Reserved Instances: example -- every Thursday between `3` PM and `6` PM
- **Spot Instances**: short workloads, much cheaper price, but can loose instance if spot price goes above your max bid (thus, less reliable)
- **Dedicated Instances**: no other customers will share your hardware
- **Dedicated Hosts**: book an entire physical server, control instance placement

**EC2 On Demand**

- Pay for what you use (billing per second -- after the first minute)
- Has the highest cost, but no upfront payment
- No long term commitment
- Recommended for short-term and un-interrupted workloads where you can't predict how the application will behave

**EC2 Reserved Instances**

- Up to 75% discount compared to On Demand
- Pay up front for what you use with long term commitment
- Reservation period can be `1` or `3` years
- Reserve a specific instance type
- Recommended for steady state usage applications (e.g., database)
- **Convertible Reserved Instance**:
  - Can change the EC2 instance type
  - Up to 54% discount
- **Scheduled Reserved Instances**:
  - Launch within the time window you reserve
  - Use when you require a fraction of day / week / month
  
**EC2 Spot Instance**

- Can get a discount of up to 90% compared to On Demand
- You may loose instance at any point if your max price is less than the current spot price (bidding approach)
- The MOST cost efficient instances in AWS
- Useful for workloads that are resilient to failure:
  - Batch jobs
  - Data analysis
  - Image processing
- Not good for mission critical jobs or databases
- Great combinatin: reserved instances for baseline needs + On Demand & Spot instances to handle peak times

**EC2 Dedicated Hosts**

- Physical dedicated EC2 server for your use
- Full control of EC2 instance placement
- Visibility into the underlying sockets / physical cores of the hardware
- Allocated to your account for a `3` year period reservation
- More expensive
- Useful for software that have complicated licensing model

**EC2 Dedicated Instances**

- Instances running on hardware that is dedicated to you
- May share hardware with other instances in same account
- No control over instance placement (can move hardware after Stop / Start)

### EC2 Elastic Network Interfaces

- Logical component in a VPC that represents a **virtual network card**
- The ENI can have the following attributes:
  - Primary private IPv4, one or more secondary IPv4
  - One Elastic IP (IPv4) per private IPv4
  - One public IPv4
  - One or more security groups
  - A MAC address
- You can create ENI independendtly and attach them on the fly on EC2 for failover
- Bound to a specific availability zone (AZ)

![](https://s3.amazonaws.com/gutucristian.com/ElasticNetworkInterface.png)

### EC2 Good to Know and Checklist

**Pricing**

- EC2 instance price (per hour) varies based on these parameters:
  - Region
  - Instance type
  - On Demand vs Spot vs Reserved vs Dedicated Host
  - Linux vs Windows vs Private OS
- You are billed by the second, with a minimum of `60` seconds
- You also pay for other factors such as storage, data transfer, fixed IP public addresses, load balancing

**Amazon Machine Image (AMI)**

- AWS comes with base images that you can use to create your EC2 instance
  - Ubuntu
  - RedHat
  - Windows
- These images can be customized at runtime using EC2 User Data
- Alternatively, we can create our own AMI that is ready to go without needing to run User Data script on start
- Building your own AMI has some advantages:
  - Pre-install packages
  - Faster boot time (no need for long EC2 User Data at boot time)
  - Machine comes configured with monitoring / enterprise software
  - Control of maintenance and updates of AMI over time
  - **Note**: AMIs are build for a specific AWS region

**EC2 Instance Characteristics Overview**

- Instances have `5` distinct characteristics:
  - RAM (type, amount, generation)
  - CPU (type, make, frequency, generation, number of cores)
  - I/O (disk performance, EBS optimizations)
  - Network (network bandwidth, network latency)
  - Graphical Processing Unit (GPU)
- R/C/P/G/H/X/I/F/Z/CR are specialized in RAM, CPU, I/O, Network, GPU
- M instance types are balanced
- T2/T3 instances are "burstable"

**Burstable Instances (T2)**

- Burst means that overall, the instance has OK CPU performance
- When the machine needs to process something unexpected (a spike in load for example), it can burst, and CPU can be VERY good
- If the machine bursts it utilizes "burst credit"
- If all the credits are gone, the CPU becomes BAD
- If machine stops bursting, credits are accumulated over time
- Burstable instances can be amazing to handle unexpected traffic, but if your instance consistently runs on low credit, you need to move to a different kind of non-burstable instance
- **Note:** it is possible to have an "unlimited burst credit balance" -- you pay extra money (can get very expensive), but you don't loose in performance

![](https://s3.amazonaws.com/gutucristian.com/BurstCredit.png)

## AWS Elastic Load Balancer (ELB) + Auto Scaling Group (ASG)

### Scalability & High Availability

- Scalability means that an application / system can handle greater loads by adapting
- There are two types of scaling:
  1. Vertical, and
  2. Horizontal (a.k.a., elasticity)
  
**Vertical Scalability**

- Vertical Scalability means increasing the hardware capabilities of the instance (scale up / scale down)
  - From: `t2.nano` - 0.5G of RAM, 1 vCPU
  - To: `u-12tb1.metal` - 12.3TB RAM, 448 vCPU
- For example, if your application is currently running on a `t2.micro` instance type, then scaling vertically could mean changing it to a `t2.large` instead
- Vertical scaling is common for **non distributed systems** like databases
- RDS, ElasticCache are services that can scale vertically
- There is a hardware limit to how much you can scale vertically

**Horizontal Scaling**

- Horizontal scaling means increasing the number of instances (scale out / scale in)
  - Auto Scaling Group (ASG)
  - Load Balancer
- Horizontal scaling implies distributed systems (e.g., Kafka, BitTorrent)

**High Availability**

- High Availability goes hand in hand with horizontal scaling
  - Auto Scaling Group multi AZ
  - Load Balancer multi AZ
- High Availability is used to guarantee **fault tolerance** -- the property that enables a system to continue operating properly in the event of the failure of some of its components
- To make our application highly available, we could run it in two Availability Zones. In this case, if one data center is compromized (e.g., natural disaster), then our application will continue to function as usual out of the second Availability Zone (data center)

### Load Balancing

- Load balancers are servers that forward internet traffic to multiple servers (EC2 Instances) downstream

![]()

**Why use a Load Balancer?**

- Spread load across multiple downstream instances
- Expose a single point of access (DNS) to your application
- Seamlessly handle failures of downstream instances
- Do regular health checks to your instances
- Provide SSL termination (HTTPS) for your websites
- Enforce stickiness with cookies
- High availability across zones
- Separate public traffic from private traffic

**Why use an EC2 Load Balancer?**

- An ELB (EC2 Load Balancer) is a **managed load balancer**
  - AWS guarantees that it will be working
  - AWS takes care of upgrades, maintenance, high availability
  - AWS provides only a few configuration knobs
- It costs less to setup your own load balancer, but it will cost more in maintenance and effort on your end
- It is integrated with many AWS offerings / services

**Health Checks**

- Health Checks are crucial for Load Balancers
- They enable the load balancer to know if instances it forwards traffic to are available to reply to requests
- The health check is done on a port and a route (`/health` is common)
- If the response is not `200 (OK)`, then the instance is marked "un-healthy"

![](https://s3.amazonaws.com/gutucristian.com/HealthCheck.png)

**Types of Load Balancers on AWS**

- **Classic Load Balancer** (v1 -- old generation) - 2009
  - HTTP, HTTPS, TCP
- **Application Load Balancer** (v2 -- new generation) - 2016
  - HTTP, HTTPS, WebSocket
- **Network Load Balancer** (v2 -- new generation) - 2017
  - TCP, TLS (secure TCP) & UDP
- It is recommended to use newer / v2 generation load balancers as they provide more features
- You can set up **internal** (private) or **external** (public) ELBs

![](https://s3.amazonaws.com/gutucristian.com/LoadBalancer.png)

**Load Balancer Security Group with Application Security Group that allows traffic only from Load Balancer**

![](https://s3.amazonaws.com/gutucristian.com/LoadBalancer.png)

**Load Balancer Good to Know**

- Load Balancers can scale but no instantaneously -- contact AWS for a "warm-up"
- Troubleshooting:
  - `4xx` errors are client induced errors
  - `5xx` errors are application induced errors
  - Load Balancer Error `503` means that LB is at capacity or no registered targets
  - If the LB can't connec to your application, check your security groups!
- Monitoring:
  - ELB access logs will log all access requests (so you can debug per request)
  - CloudWatch Metrics will give you aggregate statistics (e.g., connection counts)
