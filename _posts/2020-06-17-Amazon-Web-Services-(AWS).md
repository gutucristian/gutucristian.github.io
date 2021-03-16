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

**Step 1:** Select an instance type

The EC2 Instance Type determines the *hardware* of the host computer used for your instance. Each instance type offers different compute and memory capabilities

**Step 2:** Select an Amazon Machine Image (AMI) for your instance

The AMI is a template that contains a software configuration (for example, an operating system, an application server, and applications). From an AMI, you launch an instance, which is a *copy* of the AMI running as a virtual server in the cloud

**Step 3:** Add Storage

The **root device** for your instance **contains the image used to boot the instance**. The root device is either an Amazon Elastic Block Store (Amazon EBS) volume or an instance store volume.

Your instance may include **local storage volumes**, known as **instance store volumes**, which you can configure at launch time with **block device mapping**. After these volumes have been added to and mapped on your instance, they are available for you to mount and use. If your instance fails, or if your instance is stopped or terminated, the data on these volumes is lost; therefore, these volumes are best used for temporary (ephemeral) data. To keep important data safe, you should use a replication strategy across multiple instances, or store your persistent data in Amazon S3 or Amazon EBS volumes.

**Step 4:** Add Tags (Optional)

Tags can be used to describe or group instances

**Step 5:** Configure Security Group

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

![](https://s3.amazonaws.com/gutucristian.com/DedicatedInstanceVSDedicatedHost.png)

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

# AWS Elastic Load Balancer (ELB) + Auto Scaling Group (ASG)

## Scalability & High Availability

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

## Load Balancing

Load balancers are servers that forward internet traffic to multiple servers (EC2 Instances) downstream

![](https://s3.amazonaws.com/gutucristian.com/LoadBalancer.png)

### Why use a Load Balancer?

- Spread load across multiple downstream instances
- Expose a single point of access (DNS) to your application
- Seamlessly handle failures of downstream instances
- Do regular health checks to your instances
- Provide SSL termination (HTTPS) for your websites
- Enforce stickiness with cookies
- High availability across zones
- Separate public traffic from private traffic

### Why use an EC2 Load Balancer?

- An ELB (EC2 Load Balancer) is a **managed load balancer**
  - AWS guarantees that it will be working
  - AWS takes care of upgrades, maintenance, high availability
  - AWS provides only a few configuration knobs
- It costs less to setup your own load balancer, but it will cost more in maintenance and effort on your end
- It is integrated with many AWS offerings / services

### Health Checks

- Health Checks are crucial for Load Balancers
- They enable the load balancer to know if instances it forwards traffic to are available to reply to requests
- The health check is done on a port and a route (`/health` is common)
- If the response is **not** `200 (OK)`, then the instance is marked "un-healthy"

![](https://s3.amazonaws.com/gutucristian.com/HealthCheck.png)

## Types of Load Balancers on AWS

### Classic Load Balancer (v1 -- old generation, 2009)

- HTTP & HTTPS (**OSI layer 7**), TCP (**OSI layer 4**)
- Health checks are TCP or HTTP based
- Fixed hostname: `xxx.region.elb.amazonaws.com`
  
![](https://s3.amazonaws.com/gutucristian.com/ClassicLoadBalancer.png)

### Application Load Balancer (v2 -- new generation, 2016)

- HTTP & HTTPS (**OSI layer 7**), HTTP2, WebSocket
- Can load balance to multiple **target groups**
  - A target group is a cluster of related instances (e.g., running some HTTP application)
- Target Groups can be:
  - EC2 instances (can be managed by an Auto Scaling Group)
  - ECS tasks (managed by ECS itself)
  - Lambda functions -- HTTP request is translated into a JSON event
  - IP addresses (must be private IPs)
  - Note: ALB can route to multiple target groups and health checks are at the target group level
- As a result of target groups, ALB can load balance to multiple applications running behind the same load balancer
  - For example, we can create a listener rule such that all requests with path `/pricing/*` go to target group running the pricing microservice
  - Similarly, we can have a rule to forward requests with `/forecasting/*` in path to the target group running the forecasting EC2 instances
- ALB can also load balance to multiple applications running on the same machine (e.g., Docker containers on EC2)
- As such, ALB is a great fit for microservices and container based applications (e.g., Docker & Amazon ECS)
- In general, ALB supports traffic forwarding, redirection (e.g. from HTTP to HTTPS), and fixed responses based on conditions from a request's:
  - Path,
  - Host header
  - HTTP header
  - HTTP request method
  - Query string
  - Source IP
  - Hostname
- ALB, has a port mapping feature that enables redirection to a dynamic port in ECS
- Good to know:
  - ALB has a fixed hostname (e.g., `xxx.region.elb.amazonaws.com`)
  - The application servers **do not** see the IP of the client directly (instead Classic Load Balancers and Application Load Balancers use the private IP addresses associated with their elastic network interfaces as the source IP address for requests forwarded to your web servers)
  - The true IP of the client is inserted in the header `X-Forwarded-For`
  - We can also get the client port and protocol in the headers `X-Forwarded-Port` and `X-Forwarded-Proto` respectively
      
![](https://s3.amazonaws.com/gutucristian.com/ApplicationLoadBalancer.png)

ALB also supports SSL termination:

![](https://s3.amazonaws.com/gutucristian.com/ConnectionTermination.png)

### Network Load Balancer (v2 -- new generation, 2017)

- TCP, TLS (secure TCP), UDP (all **OSI layer 4**)
- Network load balancers (OSI layer 4) allow to:
  - Forward TCP & UDP traffic to your instances
  - Handle millions of requests per second
  - Less latency ~100 ms (vs ~400 ms fo ALB)
- **NLB has one static IP per AZ and supports assigning Elastic IP**
- NLBs are **used for extreme performance** (TCP or UDP traffic)
- You can set up **internal** (private) or **external** (public) ELBs

### Load Balancer Security Group with Application Security Group that allows traffic only from Load Balancer

![](https://s3.amazonaws.com/gutucristian.com/LoadBalancer1.png)

### Load Balancer Good to Know

- Load Balancers can scale but no instantaneously -- contact AWS for a "warm-up"
- Troubleshooting:
  - `4xx` errors are client induced errors
  - `5xx` errors are application induced errors
  - Load Balancer Error `503` means that LB is at capacity or no registered targets
  - If the LB can't connec to your application, check your security groups!
- Monitoring:
  - ELB access logs will log all access requests (so you can debug per request)
  - CloudWatch Metrics will give you aggregate statistics (e.g., connection counts)

## ELB - Stickiness

- It is possible to implement **stickiness** so that the same client is always redirected to the same instance behind a load balancer
- This is possible for **Classic Load Balancers (CLB)** and **Application Load Balancers (ALB)** -- both of which operate at **OSI layer 7**
- Stickiness is implemented via a session tracking **cookie** which has an **expiration date** that you control
- Note that enabling stickiness **may bring an imbalance to the load** over the backend EC2 instances

![](https://s3.amazonaws.com/gutucristian.com/ELBStickiness.png)

## ELB - Cross Zone Load Balancing

- With **Cross Zone Load Balancing** each load balancer instance **distributes load evenly across all registered instances in all Availability Zones (AZs)**
- Without Cross Zone Load Balancing, each load balancer would only distribute requests across registered instances in its AZ only
- **Classic Load Balancer**
  - Disabled by default
  - No charges for inter AZ data if enabled
- **Application Load Balancer**
  - Always on (can't be disabled)
  - No charges for inter AZ data
- **Network Load Balancer**
  - Disabled by default
  - You pay for inter AZ data if enabled

![](https://s3.amazonaws.com/gutucristian.com/ELBCrossZoneLoadBalancing.png)

## ELB - SSL Certificates

- An SSL Certificate allows traffic between your clients and your load balancer to be encrypted in transit (i.e., in-flight encryption)
- **SSL** refers to Secure Socket Layer -- used to encrypt connections
- **TLS** refers to Transport Layer Security which is a newer version
- Nowadays, **TLS certificates are mainly used**, but people still say SSL
- Public SSL certificates are issued by Certificate Authorities (e.g., Comodo, Symantec, GoDaddy, Digicert, etc..)
- SSL certificates have an expiration date that you set and must be renewed

![](https://s3.amazonaws.com/gutucristian.com/ELBSSL.png)

- The load balancer uses a X.509 certificate (SSL/TLS server certificate)
- You can manage certificates using ACM (AWS Certificate Manager)
- Alternatively, you can upload your own certificates
- Load Balancer HTTPS listener:
  - You must specify a default certificate
  - You can add an optional list of certs to support **multiple domains**
  - Clients can use **SNI (Server Name Identification) to specify the hostname they reach**
  - Ability to specify a security policy to support older versions of SSL / TLS for legacy clients

### SSL - Server Name Indication

- SNI solves the problem of loading **multiple SSL certificates onto one web server** (to serve multiple websites)
- It's a newer protocol and requires the client to **indicate the hostname** of the target server in the initial SSL handshake
- The server will then find the correct certificate or return the default one
- Only works for ALB & NLB (newer generation load balancers), CloudFront -- does not work for CLB

![](https://s3.amazonaws.com/gutucristian.com/ELBSSLServerNameIdentification.png)

- **Classic Load Balancer (v1)**
  - Supports only one SSL certificate
  - Must use multiple CLB for multiple hostnames with multiple SSL certificates
- **Application Load Balancer (v2)**
  - Supports multiple listeners with multiple SSL certificates
  - Uses Server Name Indication (SNI) to make it work
- **Network Load Balancer (v2)**
  - Supports multiple listeners with multiple SSL certificates

## ELB - Connection Draining

- If you are using a **Classic Load Balancer** (CLB), then it is called **Connection Draining**
- If you are using a **target group** (i.e., **Application Load Balancer** (ALB) or **Network Load Balancer** (NLB)), then it is called **Deregistration Delay**
- In either case, both are referring to the amount of time given to in-flight requests to complete before the instance is in "de-registration mode" (i.e., being stoped or terminated)
- Thus, when an instance goes into de-registration mode, the ELB stops forwarding requests to the instance and the **de-registration delay** goes into effect
- The **de-registration delay** can be **between `1` and `3600` seconds**. The **default is `300` seconds**
- Can be disabled (set value to `0`). If you do this, when an EC2 instance goes into de-registration mode all in-flight requests to that instance will be dropped
- Set to a low value if your requests are short, else set to something a bit higher so you give a chance to requests that are in-flight to be completed

![](https://s3.amazonaws.com/gutucristian.com/ELBConnectionDraining.png)

## Auto Scaling Group (ASG)

- An Auto Scaling group contains a collection of Amazon **EC2 instances that are treated as a logical grouping for the purposes of automatic scaling and management**. An Auto Scaling group also enables you to use Amazon EC2 Auto Scaling features such as health check replacements and scaling policies. Both maintaining the number of instances in an Auto Scaling group and automatic scaling are the core functionality of the Amazon EC2 Auto Scaling service
- The size of an Auto Scaling group depends on the number of instances that you set as the **desired capacity**. You can adjust its size to meet demand, either manually or by using automatic scaling
- An Auto Scaling group starts by launching enough instances to meet its desired capacity. It maintains this number of instances by performing periodic health checks on the instances in the group. The Auto Scaling group continues to maintain a fixed number of instances even if an instance becomes unhealthy. If an instance becomes unhealthy, the group terminates the unhealthy instance and launches another instance to replace it
- You can use scaling policies to increase or decrease the number of instances in your group dynamically to meet changing conditions. When the scaling policy is in effect, the Auto Scaling group adjusts the desired capacity of the group, between the minimum and maximum capacity values that you specify, and launches or terminates the instances as needed. You can also scale on a schedule
- On health checks:
  - EC2 Auto Scaling automatically replaces instances that fail health checks. If you enabled load balancing, you can enable **ELB health checks** in addition to the EC2 health checks that are **always enabled**
  - The default EC2 health checks provided by Amazon only check for hardware and software issues that may impair an instance
  - ELB health checks are more customized and can be set up to verify a TCP port on an instance is accepting connections OR a specified web page returns some success code -- thus ELB health checks are a little bit smarter and verify that actual app works instead of verifying that just an instance works
  - There is a third health check type: custom health check. If your application can't be checked by simple HTTP request and requires advanced test logic, you can implement a custom check in your code and set instance health though an API ([reference](https://docs.aws.amazon.com/autoscaling/ec2/userguide/healthcheck.html))
  - ELB health checks used in conjunction with an ASG allows ELB health check to report application health on the instances to ASG and ASG will then rotate failing instances (ELB health checks can be pointed at a specific URL similar to Route 53 health checks)
- ASG Health Check Grace Period: Amazon EC2 Auto Scaling doesn't terminate an instance that came into service based on EC2 status checks and ELB health checks until the health check grace period expires ([reference](https://aws.amazon.com/premiumsupport/knowledge-center/auto-scaling-terminate-instance/))
- Thus, ASGs allow us to:
  - Scale out (add EC2 instances) to match an increase in load
  - Scale in (remove EC2 instances) to match a decrease in load
  - Ensure we have a minimum and maximum number of machines running
  - Automatically register new instances to a load balancer

![](https://s3.amazonaws.com/gutucristian.com/AutoScalingGroup.png)

### Auto Scaling Group with Load Balancer

![](https://s3.amazonaws.com/gutucristian.com/ELBWithASG.png)

### ASGs Have The Following Attributes

- A launch configuration
  - AMI + Instance Type
  - EC2 User Data
  - EBS Volumes
  - Security Groups
  - SSH Key Pair
- Minimum Size / Maximum Size / Initial Capacity
- Network + Subnets Information
- Load Balancer Information
- Scaling Policies

### Auto Scaling Alarms

- ASG scaling policies are driven by CloudWatch alarms
- An Alarm monitors a metric. For example:
  - Average CPU
  - Number of requests on the ELB per instance
  - Average Network In
  - Average Network Out
- Metrics are computed for the overall ASG instances
- Based on the alarm:
  - We can create **scale-out** policies (increase the number of instances)
  - We can create **scale-in** policies (decrease the number of instances)
- We can also auto scale based on a **custom metric**:
  - Send custom metric from application on EC2 to CloudWatch (using `PutMetric` API)
  - Create CloudWatch alarm to react to low / high values
  - Use the CloudWatch alarm as the scaling policy for ASG

![](https://s3.amazonaws.com/gutucristian.com/ASGAlarm.png)

### ASG Additional Information

- Scaling policies can be based on predefined metrics (e.g., CPU, Network, etc..) and even on custom metrics or based on a schedule (if you know the user patterns)
- To update an ASG, you must provide a new launch configuration / launch template
- IAM roles attached to an ASG will get assigned to EC2 instances
- ASGs are free -- you pay for the underlying resources being used
- Having instances under an ASG means that if they get terminated for whatever reason, the ASG will automatically create new ones as a replacement
- ASG can terminate instances marked as unhealthy by an LB and replace them

### Auto Scaling Groups -- Scaling Policies

- **Target Tracking Scaling**
  - With target tracking scaling policies, you select a scaling metric and set a target value. Amazon EC2 Auto Scaling creates and manages the CloudWatch alarms that trigger the scaling policy and calculates the scaling adjustment based on the metric and the target value
  - Example: I want the average ASG CPU to stay at around 40%
- **Simple / Step Scaling**
  - With step scaling and simple scaling, you choose scaling metrics and threshold values for the CloudWatch alarms that trigger the scaling process. You also define how your Auto Scaling group should be scaled when a threshold is in breach for a specified number of evaluation periods
- **Scheduled Actions**
  - Scheduled scaling allows you to set your own scaling schedule
  - For example, let's say that every week the traffic to your web application starts to increase on Wednesday, remains high on Thursday, and starts to decrease on Friday. You can plan your scaling actions based on the predictable traffic patterns of your web application. Scaling actions are performed automatically as a function of time and date

**Simple Scaling Example**

- You pick **ANY** Cloud Watch metric
- For this examples I am choosing CPU utilization
- You specify, a **SINGLE THRESHOLD** beyond which you want to scale and specify your response
- Example: how many EC2 instances do you want to add or take away when the CPU utilization breaches the threshold
- The scaling policy then acts
- THRESHOLD: add `1` instance when CPU utilization is between `40%` and `50%`
- Note: This is the **ONLY** threshold

**Step Scaling Example**

- You specify **MULTIPLE** thresholds along with different responses
- Threshold `A`: add 1 instance when CPU utilization is between `40%` and `50%`
- Threshold `B`: add 2 instances when CPU utilization is between `50%` and `70%`
- Threshold `C`: add 3 instances when CPU utilization is between `70%` and `90%`
- And so on and so on
- Note: There are **MULTIPLE** thresholds

**Target Tracking Example**

- You don't want to have to make so many decisions
- Makes the experience simple as compared to the previous 2 scaling options
- It’s automatic
- All you do is pick metric (CPU utilization in this example)
- Set the value and that’s it
- Auto scaling does the rest adding and removing the capacity in order to keep your metric (CPU utilization) as close as possible to the target value
- It’s **SELF OPTIMIZING**: meaning that it has an algorithm that learns how your metric changes over time and uses that information to make sure that over and under scaling are minimized

### Auto Scaling Groups -- Scaling Cooldowns

- The cooldown period helps to ensure that your Auto Scaling Group doesn't launch or terminate additional instances before the previous scaling activity takes effect
- In addition to default cooldown for Auto Scaling Group, we can create cooldowns that apply to a specific **simple scaling policy**
- A scaling-specific cooldown period overrides the default cooldown period
- One common use for scaling-specific cooldowns is with a scale-in policy -- a policy that terminate instances based on a specific criteria or metric. Because this policy terminates instances, Amazon EC2 Auto Scaling needs less time to determine whether to terminate additional instances
- If the default cooldown period of `300` seconds is too long, you can reduce costs by applying a scaling-specific cooldown period of `180` seconds to the scale-in policy
- If your application is scaling up and down multiple times each hour, modify the Auto Scaling Group cool-down timers and the CloudWatch Alarm Period that triggers the scale in

![](https://s3.amazonaws.com/gutucristian.com/ScalingActionWithCooldown.png)

# EC2 Storage -- Elastic Block Storage (EBS) and Elastic File System (EFS)

## What is an EBS volume?

- An EBS volume is a **network** drive you can attach to your instance
  - It uses the network to communicate to the instance (which implies some latency)
- EBS volumes persist independently from the running life of an EC2 instance. However, you can choose to automatically delete an EBS volume when the associated instance is terminated
- It is locked to an Availability Zone (AZ)
  - An EBS volume in `us-east-1a` cannot be attached to an instance in `us-east-1b`
  - To move a volume across AZs you first need to create a **snapshot** of it
- Has provisioned capacity (size in GBs and IOPS - Input Output Per Second)
  - You get billed for all the provisioned capacity
  - You can increase the capacity of the drive over time
- Read [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html) how to mount an Amazon EBS volume and make it available for use
- Throughput vs IOPS: IOPS measures the number of read and write **operations** per second, while throughput measures the number of **bits** read or written per second

![](https://s3.amazonaws.com/gutucristian.com/ElasticBlockStorage.png)

## EBS Volume Types

- EBS volumes come in `4` types:
  - `gp2` (SSD): general purpose SSD volume that balances price and performance for a wide variety of workloads
  - `io1` (SSD): highest performance SSD volume for mission critical low latency or high throughput workloads
  - `st1` (HDD): low cost HDD volume designed for frequently accessed, throughput intensive workloads
  - `sc1` (HDD): lowest cost HDD volume designed for less frequently accessed workloads
- EBS volumes are characterized by: size, throughput, IOPS
- **Only `gp2` and `io1` can be used as boot volumes**

### EBS `gp2` (SSD) Volume Type

- General purpose SSD volume that balances price and performance for a wide variety of workloads
- Recommended for most workloads
- Virtual Desktops
- Low-latency interactive apps
- Development and test environments
- Small GP2 volumes can burst IOPS to `3000`
- Size range: `1` GiB - `16` GiB
- Max IOPS is `16000`
- `3` IOPS per GB -- which means that at `5334` GB we are at max IOPS
- Throughput not applicable

### EBS `io1` (SSD) Volume Type

- Highest performance SSD volume for mission critical low latency or high throughput workloads
- Critical business applications that require sustained IOPS performance or more than `16000` IOPS per volume (gp2 limit)
- Large database workloads (e.g., MongoDB, Cassandra, PostgreSQL, etc..)
- Size range: `4` GiB - `16` TiB
- IOPS is provisioned: MIN `100` - MAX `32000` or `64000` if on Nitro instance
- The maximum ratio of provisioned IOPS to requested volume size (in GiB) is 50:1
- Throughput not applicable

### EBS `st1` (HDD) Volume Type

- Low cost HDD volume designed for frequently accessed, throughput intensive workloads
- Streaming workloads requiring consistent, fast throughput at low price
- Big data, data warehouses, log processing
- Apache Kafka
- Cannot be a boot volume
- Size range: `500` GiB - `16` TiB
- Max IOPS is `500`
- Baseline `40` MB/s per TiB throughput until a max of `500` MiB/s  -- can burst

### EBS `sc1` (HDD) Volume Type

- Lowest cost HDD volume designed for less frequently accessed workloads
- Throughput oriented storage for large volumes of data that is infrequently accessed
- Scenarios where the lowest storage cost is important
- Cannot be a boot volume
- Size range: `500` GiB - `16` TiB
- Max IOPS is `250`
- Baseline `12` MB/s per TiB throughput until a max of `250` MiB/s  -- can burst

## EBS vs Instance Store

- Some instances do not come with Root EBS volumes, instead they come with "**Instance Store**" (i.e., ephemeral storage)
- **Instance store is physically attached** to the machine whereas **EBS is a network device**
- Instance Store pros:
  - Better IO performance (very high IOPS -- since it is physically attached to the instance)
  - Good for caching (since its closer to hardware executing our code)
- Instance Store cons:
  - Data survives only **reboots**
  - **If instance is stopped or terminated the instance store is lost**
  - You cannot resize the instance store
  - Performing backups is left to the user

## Elastic File System (EFS)

- Managed network file system (NFS) that can be mounted on many EC2 instances at once
- EFS works with EC2 instances in multi-AZ (which means its expensive)
- Highly available, pay per use (which is different than EBS where you pay for amount of storage you reserve)
- Uses NFSv4.1 protocol
- Uses security group to control access to EFS
- **Compatible with Linux based AMIs (not Windows)**
- Can leverage encryption at rest using KMS
- POSIX file system that has standard file API
- File system scales automatically, pay per use, so no capacity planning
- EFS scale:
  - Supports thousands of concurrent NFS clients with `10` GB+ /s throughput
  - Can grow to Petabyte-scale network file system automatically (again: no capacity planning, pay per use)
- To mount an EFS file system you will need to install the `amazon-efs-utils`. Follow instruction [here](https://docs.aws.amazon.com/efs/latest/ug/mounting-fs.html)

![](https://s3.amazonaws.com/gutucristian.com/ElasticFileSystem.png)


### EFS Performance modes (set at EFS creation time):

- General purpose (default): low latency. In General Purpose performance mode, read and write operations consume a different number of file operations. Read data or metadata consumes one file operation. Write data or update metadata consumes five file operations. A file system can support up to 35,000 file operations per second (use cases: web server, CMS)
- Max I/O: higher latency. Max I/O performance mode doesn't have a file system operations limit. Use the Max I/O performance mode if you have a very high requirement of file system operations per second
- Note: General Purpose performance mode has the lower latency of the two performance modes and is suitable if your workload is sensitive to latency. Max I/O performance mode offers a higher number of file system operations per second but has a slightly higher latency per each file system operation

### EFS Throughput modes (set at EFS creation time):

- Bursting: throughput scales with file system size
- Provisioned: throughput fixed at specified amount (range: `1` - `1024` MiB/s)

### EFS Storage Tiers (file lifecycle management feature -- move file if unused for `N` days):
  
- Standard storage tier: for frequently accessed
- Infrequent Access storage tier (EFS IA): after not used for `N` days file is moved to infrequent storage class, this results is a lower price, but cost to retrive is higher
  
## EBS vs EFS

- EFS is for a shared network file system to be mounted across many instances across AZs
- EBS is for a network file system that can be attached to only one instance and is locked at AZ level (meaning you can't decide to detach it from an instance in `us-east-1a` and attach it to another instance in `us-east-1b`)
- Instance Store is to get the maximum amount of I/O (since it is storage that is physically attached to our instance) but it is something you loose once instance is terminated so it is an ephemeral drive

### EBS

- Can be attached to only one instance at a time
- Are locked at the Availability Zone (AZ) level (e.g., `us-east-1a`)
- `gp2` volume type: I/O increases if the disk size increases
- `io1`: can increase I/O independently
- To migrate an EBS volume across AZs:
  - Take a snapshot
  - Restore the snapshot to another AZ
  - EBS backups use I/O and you shouldn't run them while your application is handling a lot of traffic since you will be affecting performance (since IOPS is limited)
- **Root** EBS volumes of instances get terminated by default if the EC2 instance get terminated but you can disable that

![](https://s3.amazonaws.com/gutucristian.com/EBSSnapshot.png)

### EFS

- Supports mounting thousands of instances across AZs
- Only supports for POSIX based instances (e.g., Linux)
- More expensive than EBS (because of the cross AZ communication required to have a shared file system across AZs)
- Can leverage EFS Infrequent Access storage class for files not accessed after `N` days for a much cheaper storage cost

![](https://s3.amazonaws.com/gutucristian.com/EFS1.png)

# AWS Relational Databases (RDS), Aurora, ElastiCache

- RDS stands for Relational Database Service
- It is an AWS service that manages relational databases (i.e., databases that use SQL as a query language)
- Thus, it allows you to create databases in the cloud that are managed by AWS. For example:
  - MySQL
  - PostgreSQL
  - MariaDB
  - Oracle
  - Microsoft SQL
  - Aurora (AWS proprietary database)

## Advantages of using RDS over deploying same database on EC2

- Automated provisioning, OS patching
- Continuous backups and ability to **restore** to a specific timestamp (**Point in Time Restore**)
- Monitoring dashboards
- **Read replicas** for improved read performance
- **Multi AZ** setup for **disaster recovery** (DR)
- Scaling capability (vertical and horizontal)
- Storage backed by EBS (`gp2` or `io1` -- both SSD)
- **Note:** because this is a **managed service** you **cannot** SSH into the underlying instance

## RDS Backups

- Backups are **automatically enabled** in RDS
- Automated backups:
  - Daily full backup of the database (during the **maintenance window**)
  - Database **transaction logs** are backed-up by RDS **every five minutes**, thus enabling the ability to restore to any point in time (from oldest backup to the one that was done five minutes ago)
  - Default `7` day backup retention period, but can be increased to `35` days
- DB Snapshots:
  - Different from automated backups in that these are **manually triggered** by the user
  - Retention of backup can be as long as you want

## RDS Read Replicas for read scalability

- Read replicas are **read only** database replicas that can be used by applications to **read** data. This pattern allows us to reduce load on the main database instance which is **read write** enabled
- We can create up to `5` read replicas which can be within AZ, across AZs, or across regions
- The replication itself is **asynchronous** which means that the read replicas are **eventually consistent**. Eventually consistent means that they **eventually** reflect the data in the master database instance, but not necessarily right away
- Replicas can be **promoted** to become their own separate database
- Applications must update the connection string to leverage read replicas. In other words, we have **separate endpoints** for the read replicas and the main RDS DB instance

![](https://s3.amazonaws.com/gutucristian.com/RDSReadReplicas.png)

### RDS Read Replicas Use Cases

- You have a production database that is taking normal load
- A separate team comes in and wants to use the data to run some reporting application
- In this scenario, we can create an RDS Read Replica of our main DB instance and tell the other team to use that instance for their reporting application instead
- As such, our production application and database is completely unaffected
- **Note:** read replicas are used for **read operations only** (so `SELECT` kind of statements **not** `INSERT, UPDATE, DELETE`)

![](https://s3.amazonaws.com/gutucristian.com/RDSReadReplicas1.png)

### RDS Read Replicas

- In AWS there is a network cost when data goes from one AZ to another
- To reduce costs keep your Read Replicas in the same AZ

![](https://s3.amazonaws.com/gutucristian.com/ReadReplicaInSameAZ.png)

### RDS Multi AZ (Disaster Recovery)

- With Multi AZ, RDS maintains a **standby** DB instance (in AZ B from figure below) that is a **replica** of the master DB instance (in AZ A in instance below)
- Multi AZ has **synchronous replication** from master DB instance to standby instance
- Application uses **one DNS name** to write and read from DB
- In case the master DB instance fails RDS will (behind the scenes) failover to the standby instance and the DNS name will be updated to resolve to the failover -- thereby leaving the application completely **unaffected** and **without any need for developer intervention**
- Because the standby instance is in another AZ, we increase our applications **availability** and enable **fault tolerance** in case of loss of one AZ
- **Note:** RDS Read Replicas can be setup as Multi AZ for Disaster Recovery

![](https://s3.amazonaws.com/gutucristian.com/RDSMultiAZAutomaticFailover.png)

## RDS Encryption + Security

- At rest encryption:
  - Possibility to encrypt the master & read replicas with AWS KMS - AES-256 encryption
  - Encryption has to be defines at launch time
  - **If the master is not encrypted, then the read replicas cannot by encrypted**
  - Transparent Data Encryption (TDE) available for Oracle and SQL Server (an alternative way of encrypting your database)
- In-flight encryption:
  - SSL certificates to encrypt data to RDS in flight
  - Provide SSL options with trust certificate when connecting to database
  - To enforce SSL in **PostgreSQL** set `rds.force_ssl=1` in the AWS RDS Console (Parameter Groups) and in `MySQL` within the DB run `GRANT USAGE ON *.* TP 'mysqluser'@'%' REQUIRE SSL;` (SQL command)

### RDS Encryption Operations

- Encrypting RDS backups
  - Snapshots of un-encrypted RDS databases are un-encrypted
  - Snapshots of encrypted RDS databases are encrypted
  - Can copy an un-encrypted snapshot into an encrypted one (using some options)
- **To encrypt an un-encrypted RDS database:**
  - Create a snapshot of the un-encrypted database
  - Copy the snapshot and enable encryption for the snapshot
  - Restore the database from the encrypted snapshot
  - Migrate applications to the new database and delete the old one

### RDS Security - Network & IAM

- Network security
  - RDS databases are usually deployed within a **private subnet**, not in a public one
  - RDS security works by leveraging security groups (the same concept as for EC2 instances) which control which IP / security group can **communicate** with RDS
- Access Management
  - IAM policies help control who can **manage** AWS RDS (through the RDS API)
  - Traditional username and password can be used to **login into** the database
  - **IAM-based authentication** can be used to login into RDS MySQL and PostgreSQL

### RDS -- IAM Authentication

- IAM database authentication works with **MySQL** and **PostgreSQL**
- You don't need a password just an authentication token obtained through IAM & RDS API calls
- Auth token has a lifetime of `15` minutes
- Benefits of this approach:
  - Network in/out must be encrypted using SSL
  - IAM is used to **centrally manage** users instead of DB
  - Can leverage IAM Roles and EC2 instance profiles for easy integration

![](https://s3.amazonaws.com/gutucristian.com/RDSIAMAuthentication.png)

### RDS Security -- Summary

- Encryption at rest:
  - Is done only when you first create the DB instance, or
  - Unencrypted DB => snapshot => copy snapshot as encrypted => create encrypted DB from snapshot
- Your responsability:
  - Check the ports / IP / security group inbound rules in DB's security group
  - In-database user creation and permissions or manage through IAM
  - Creating a database with or without public access
  - Ensure parameter groups or DB is configured to only allow SSL connections
- AWS responsability:
  - Guarantees nobody can SSH to your DB instance
  - Performs all required DB and OS patching

## Aurora Overview

- Aurora is a proprietary technology from AWS (not open source)
- PostgreSQL and MySQL are both supported as Aurora DB (that means your drivers will work as if Aurora was a PostgreSQL or MySQL database)
- Aurora is "AWS cloud optimized" and claims 5x performance improvement over MySQL on RDS and over 3x the performance of PostgreSQL on RDS
- Aurora has **shared storage volume** that **automatically grows** in increments of `10` GB, up to `64` TB
- Aurora can have `15` replicas (while MySQL can only have `5`) and the replication process is faster (sub `10` ms replica lag)
- Failover in Aurora is instantaneous
- Aurora costs ~20% more than RDS

### Aurora High Availability and Read Scaling

- Store `6` copies of you data anytime you write anything across `3` AZs
  - `4` copies out of `6` needed for writes
  - `3` copies out of `6` needed for reads
  - Self healing with peer-to-peer replication
  - Storage is **striped** across hundreds of volumes
- One Aurora instance takes all the writes (i.e., the master instance)
- Automated failover from master in less than `30` seconds
- Master plus up to `15` Aurora **read replicas**
- Support for **cross region replication**

![](https://s3.amazonaws.com/gutucristian.com/AuroraOverview.png)

### Aurora DB Cluster

- Aurora provides a **constant** writer endpoint that always points to the master database instance -- even if the master DB instance changes (e.g., during a failover event) the **writer endpoint stays the same** requiring **no intervention**
- We also have many **read replicas** which can be enabled to have **auto scaling**. Now, because of auto scaling, it can be really hard for applications to keep track where are the read replicas, connection url, etc.. So, Aurora introduced the **reader endpoint** concept which exposes a constant DNS endpoint (which is actually a **load balancer**) that we can use to perform all reads

![](https://s3.amazonaws.com/gutucristian.com/AuroraDBCluster.png)

### Aurora Security

- Similar to RDS since it uses the same engine
- Encryption at rest using KMS
- Automated backups, snapshots, and replicas are also encrypted
- Encryption in flight using SSL (same process as MySQL and Postgres)
- **Possibility to authenticate using IAM token (as in RDS)**
- You are responsible for protecting the instance with security groups
- You can't SSH into the underlying Aurora instances

### Aurora Serverless

- Automated database instantiation and auto-scaling based on actual usage
- Good for infrequent intermittent or unpredictable workloads
- No capacity planning needed
- Pay per second, can be more cost-effective

![](https://s3.amazonaws.com/gutucristian.com/AuroraServerless.png)

### Aurora Global

- **Aurora Cross Region Read Replicas**:
  - Useful for disaster recovery
  - Simple to put in place
- **Aurora Global Database (recommended)**:
  - **`1` primary region** (for read / write)
  - **Up to `5` secondary read only regions** where replication lag is less than `1` second
  - **Up to `16` Read Replicas per secondary region``
  - Helps to decrease latency
  - Promoting another region (for DR) has a recovery time objective (RTO) of less than one minute

![](https://s3.amazonaws.com/gutucristian.com/AuroraGlobal.png)

## AWS ElastiCache Overview

- The same way RDS is to get a managed relational database, ElastiCache is to get a **managed Redis** or **Memcached**
- Caches are in-memory databases with really high performance, low latency
- Caches are often used to help reduce load off of databases for read intensive workloads
- Helps make your application stateless
- Has **write scaling** capability using **sharding**
- Has **read scaling** capability using **read replicas**
- Has **Multi AZ failover capability**
- AWS takes care of OS maintenance / patching, optimizations, setup, configurations, monitoring, failover recovery, backups

### ElastiCache Solution Architecture -- DB Cache

- Applications queries ElastiCache. If it does not have the data ("cache miss"), then application queries RDS and stores result in ElastiCache, else it uses the data available from ElastiCache ("cache hit")
- This design helps **relieve load** in RDS
- Cache must have an **invalidation strategy** to make sure only the most current data is kept in the cache (since we are limited in cache size)

![](https://s3.amazonaws.com/gutucristian.com/ElastiCacheLazyLoading.png)

### ElastiCache Solution Architecture -- User Session Store

- User logs in to any of the application instances
- The application writes the session data into ElastiCache
- When the user hits another instance of our application the instance retrieves the data and user info (e.g., if user is already logge in) from ElastiCache

![](https://s3.amazonaws.com/gutucristian.com/ElastiCacheSessionStorage.png)

### ElastiCache Common Uses

- To relieve read load from a database
- To share some application state

### ElastiCache -- Redis vs Memcached

- **Redis (Replication)**
  - Has **Multi AZ auto-failover feature** to make it **highly available**
  - Has **read replicas** to scale reads
  - Data durability using AOF persistence -- meaning even if your cache is stopped and then restarted you can still have the data from before stopping available to you
  - **Backup and restore features**
- **Memcached (Sharding)**
  - Uses multiple nodes for partitioning of data (sharding)
  - **Non persistent**
  - **No backup and restore**
  - Multi-threaded architecture

## ElastiCache Strategies

### Caching Implementation Considerations

Read in more detail [here](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Strategies.html)

- **Is caching effective for that data?**
  - Caching will be effective if data is changing slowly and few keys are frequently needed
  - Caching will not be effective if data is changing rapidly many keys are frequenly needed.
- **Is data structured well for caching?**
  - Key value based data works great for caching

### Lazy Loading / Cache Aside / Lazy Population Caching Strategy

- With **lazy loading** pattern, application first checks for data in cache
- If the data is there then we take it and use it -- this is called a **cache hit**
- If the data is not in the cache, then this is called a **cache miss**
- If there is a **cache miss** the application code does two things: (1) makes a call to the database to retrive the data and (2) updates the cache with this new data, so that the next time we ask for it its there
- Pros of this approach:
  - Only requested data is cached (the cached is not filled up with unused data)
  - Node failures are not fatal (there is just an increased latency to **warm up** the cache again will all of our data)
- Cons of this approach:
  - **Read penalty**: cache miss results in `3` round trips -- which may be a noticeable delay for that request
  - Stale data: data could be updated in the database and outdated in the cache

![](https://s3.amazonaws.com/gutucristian.com/ElastiCacheLazyLoading.png)

### Write Through Caching Pattern

- With **write through** pattern, whenever the application writes data it will first write to the cache and then to the database
- Pros:
  - The data in the cache is **never** stale
- Cons:
  - **Write penalty**: each write requires `2` calls (still, this might not be so bad since users generally accept that writes may take longer than reads)
  - There may be missing data in the cache since we only actually write to the cache when we have a need to write to the database (this can be mitigated by combining write through with lazy loading strategy)
  - This design is prone to **cache churn**: the idea that we may end up writing a lot of data which might never be read

![](https://s3.amazonaws.com/gutucristian.com/ElastiCacheWriteThrough.png)

### Cache Evictions and Time To Live (TTL)

- Cache evictions can occur in three ways:
  - You delete the item explicitly
  - Item is evicted because the memory is full and it is least recently used (LRU cache)
  - The Time To Live for the item expired

# Route 53

## AWS Route53 Overview

- Route 53 is a managed DNS (Domain Name System)
- DNS is a collection of rules and records which helps clients understand how to reach a server through its domain name
- In AWS, the most common records are:
  - `A`: hostname to IPv4
  - `AAAA`: hostname to IPv6
  - `CNAME`: hostname to hostname
  - `Alias`: hostname to AWS resource
- Route 53 can use:
  - Public domain names you own (or buy)
  - Private domain names that can be resolved by your instance in your VPCs
- Route53 has advanced features such as:
  - Load balancing (through DNS -- also called client side load balancing)
  - Health checks (although limited)
  - Routing policies: simple, failover, geolocation, latency, weighted, multi value
- You pay $0.50 per month per hosted zone

![](https://s3.amazonaws.com/gutucristian.com/Route53Overview.png)

## DNS Records TTL (Time to Live)

- When we make a DNS request to Route 53 to resolve a certain domain, Route 53 will send back to us the IP as well as a TTL (that is predefined by us in Route 53)
- The web browser will cache the received IP for the requested hostname for the duration of the TTL
- After the TTL is over, if we receive another DNS request for the hostname, then the browser will query Route 53 which will (again) return to us the IP mapped to this domain (which *may* have changed) as well as the TTL
- A high TTL would be something like `24` hours
  - The downside of a high TTL is that there is a chance that the IP cached by the browser may be out of date for a long time before the TTL expires and the new one is received
  - The upside of a high TTL is that it significantly reduces traffic on the DNS server
- A low TTL would be something like `60` seconds
  - The downside of this is that it increases traffic to DNS server significantly
  - The upside is that records are outdated for less time (in case the IP mapping for the hostname changes)
- TTL is mandatory for each DNS record

![](https://s3.amazonaws.com/gutucristian.com/Route53TTL.png)

## `CNAME` vs `Alias` Record

- For example, lets say we have some AWS resource (e.g., Load Balancer, CloudFront, etc..) that exposes an AWS hostname: `lb1-1234.us-east-2.elb.amazonaws.com` and we want the hostname `myapp.mydomain.com` to point to it
- In this case we can use a `CNAME` record:
  - `CNAME` can point any **non-root** (also called **non apex**) hostname to any other hostname (e.g., `app.mydomain.com` => `blabla.anything.com`)
  - Again, `CNAME` records only work for **non root** domains (e.g., `abc.mydomain.com`)
- We can also use an `Alias` but **note:**
  - An `Alias` record can only point a hostname to an **AWS resource** (e.g., `abc.mydomain.com` => `lb1-1234.us-east-2.elb.amazonaws.com`)
  - The benefit of an `Alias` record is that it works for **root domains** and also **non-root domains**
  - `Alias` records are also **free of charge**
  - Has native health check

## Simple Routing Policy

- Use when you need to redirect to a **single resource**
- You **cannot** attach health checks to a simple routing policy
- If multiple values are returned (i.e., IPs), a **random** one is chosen **by the client** -- so there is **client side load balancing**

![](https://s3.amazonaws.com/gutucristian.com/Route53SimpleRouting.png)

## Weighted Routing Policy

- Control the percent of the requests that go to a specific endpoint
- For example, can be used to test a certain percent of traffic to a new app version
- Helpful to split traffic between regions
- Can be associated with health checks

![](https://s3.amazonaws.com/gutucristian.com/Route53WeightedRouting.png)

## Latency Routing Policy

- Redirect user to the server that would result in least latency
- For example, a user in Germany could be redirected to an instance in the US if AWS Route53 decided that it would yield the lowest latency
- Can be associated with health checks

![](https://s3.amazonaws.com/gutucristian.com/Route53LatencyRoutingPolicy.png)

## Failover routing

- Use when you want to configure active-passive failover
- Failover routing lets you route traffic to a resource when the resource is healthy or to a different resource when the first resource is unhealthy. The primary and secondary records can route traffic to anything from an Amazon S3 bucket that is configured as a website to a complex tree of records

![](https://s3.amazonaws.com/gutucristian.com/Route53FailoverRouting.png)

## Geo Location Routing Policy

- Geolocation routing lets you choose the resources that serve your traffic based on the geographic location of your users, meaning the location that DNS queries originate from. For example, you might want all queries from Europe to be routed to an ELB load balancer in the Frankfurt region
- We also need to create a default policy (in case there is no match)

![](https://s3.amazonaws.com/gutucristian.com/Route53GeoLocationRoutingPolicy.png)

## Multi Value Routing Policy

- Use when you want Route 53 to respond to DNS queries with up to `8` healthy records selected at random
- Multivalue answer routing lets you configure Amazon Route 53 to return multiple values, such as IP addresses for your web servers, in response to DNS queries. You can specify multiple values for almost any record, but multivalue answer routing also lets you check the health of each resource, so Route 53 returns only values for healthy resources. It's not a substitute for a load balancer, but the ability to return multiple health-checkable IP addresses is a way to use DNS to improve availability and load balancing
- To route traffic approximately randomly to multiple resources, such as web servers, you create one multivalue answer record for each resource and, optionally, associate a Route 53 health check with each record. Route 53 responds to DNS queries with up to `8` healthy records and gives different answers to different DNS resolvers. If a web server becomes unavailable after a resolver caches a response, client software can try another IP address in the response
- Note the following:
  - If you associate a health check with a multivalue answer record, Route 53 responds to DNS queries with the corresponding IP address only when the health check is healthy
  - If you don't associate a health check with a multivalue answer record, Route 53 always considers the record to be healthy
  - If you have eight or fewer healthy records, Route 53 responds to all DNS queries with all the healthy records
  - When all records are unhealthy, Route 53 responds to DNS queries with up to eight unhealthy records

![](https://s3.amazonaws.com/gutucristian.com/Route53MultiValueRoutingPolicy.png)

## Health Checks

- If you want Route 53 to check the health of a specified endpoint and to respond to DNS queries using the record only when the endpoint is healthy, choose a health check
- Note if, for example, you have a weighted routing policy with multiple endpoints and you want to use health checks, then you will need to define a Route53 health check for each one of those endpoints
- After `N` failed health checks instance is considered unhealthy (default is `3`)
- After `N` health checks passed instance is considered healthy (default `3`)
- Default health check interval is `30` seconds (can be set to `10` seconds but is more expensive)
- **About 15 health checkers will check the endpoint we define** which means about one request every `2` seconds
- Can have HTTP, TCP, and HTTPS health checks (no SSL verification)
- We can also integrate health check with CloudWatch

# VPC Fundamentals

- VPC, Subnets, Internet Gateways, NAT Gateways
- Security Groups, Network ACL (NACL), VPC Flow Logs
- VPC Peering, VPC Endpoints
- Site to Site VPN & Direct Connect

![](https://s3.amazonaws.com/gutucristian.com/VPCSubnet.png)

## VPC & Subnets

- **VPC:** private network to deploy your resources (regional resources)
- **Subnets:** allow you to partition your network inside your VPC (subnets are defined at the Availability Zone level)
- A **public subnet** is a subnet that is accessible from the internet
- A **private subnet** is a subnet that is **not** accessible from the internet
- To define access to the internet and **between subnets** we use **Route Tables**

![](https://s3.amazonaws.com/gutucristian.com/VPCSubnetsDiagram.png)

## Internet Gateway and NAT Gateways

- **Internet Gateways** help our VPC instances connect to the internet
- Public subnet has a route to the internet gateway
- **NAT Gateways** (AWS managed) and **NAT Instances** (self managed) allow your instances in your **Private Subnets** to access the internet while remaining private

![](https://s3.amazonaws.com/gutucristian.com/InternetGatewaysAndNATGateways.png)

## Network ACL and Security Groups

- **NACL** (Network ACL)
  - A firewall which controls the traffic from and to the subnet (i.e., the **first mechanism of defense of our public subnet**)
  - Can have **ALLOW** and **DENY** rules
  - Are attached at the **Subnet** level
  - Rules **only** include **IP addresses**
- **Security Groups**
  - A firewall that controls the traffic to and from an **ENI** or an **EC2 Instance** (i.e., **second mechanism of defense**)
  - Can have **only ALLOW** rules
  - Rules can include IP addresses as well as other security groups

![](https://s3.amazonaws.com/gutucristian.com/NetworkACLAndSecurityGroups.png)

## NACL vs Security Groups

![](https://s3.amazonaws.com/gutucristian.com/NACLvsSG.png)

## VPC Flow Logs

- Capture information about network traffic:
  - **VPC** Flow Logs
  - **Subnet** Flow Logs
  - **Elastic Network Interface** Flow Logs
- Captures network information from AWS managed interfaces too: Elastic Load Balancers, ElastiCache, RDS, Aurora, etc..
- VPC Flow logs data can go to S3 / CloudWatch Logs

## VPC Peering

- Connect two VPC privately using AWS network
- Make them behave as if the two VPCs are in the same network 
- We do this by setting up a **VPC peering connection** between them
- The two VPCs must **not** have overlapping CIDR (IP address range)
- VPC Peering is **not transitive**. If we have a peering connection between (VPC A and VPC B) and (VPC A and VPC C) this does not mean that VPC C can communicate with VPC B (this means there is **no transitivity**)

![](https://s3.amazonaws.com/gutucristian.com/VPCPeering.png)

## VPC Endpoints

- **Use when you need private access from within your VPC to an AWS services**
- Endpoints allow you to connect to AWS services **using a private network** instead of the public network
- This gives you increased security and lower latency to access AWS services
- Use **VPC Endpoint Gateway** for S3 and DynamoDB
- Use **VPC Endpoint Interface** for the rest of the services

![](https://s3.amazonaws.com/gutucristian.com/VPCEndpoints.png)

## Site to Site VPN & Direct Connect

- **Site to Site VPN**
  - Connect an on premise VPN to AWS
  - The connection is **automatically encrypted**
  - Goes over the **public internet**
- Direct Connect (DX)
  - Establish a **physical connection** between on-premise and AWS
  - The connection is private secure and fast
  - Goes over a private network
- **Note:** Site to site VPN and Direct Connect cannot access VPC endpoints

## Three Tier Solution Architecture

- The `3` tiers:
  - Public Subnet (e.g., ELB which is publicly available by users)
  - Private Subnet (e.g., EC2 instaces with ASG sitting behind the ELB -- they do not need to be accessible to anyone beside the ELB, so we use route tables to define network access between public ELB subnet and the EC2 private subnet)
  - Data Subnet (e.g., AWS RDS + ElastiCache also sitting in private subnet

![](https://s3.amazonaws.com/gutucristian.com/ThreeTierArchitecture.png)

## VPC Cheatsheet

- **VPC:** Virtual Private Cloud
- **Subnets:** tied to an AZ, network partition of the VPC
- **Internet Gateway:** at the VPC level, provide Internet Access
- **NAT Gateway / Instances:** give internet access to private subnets
- **NACL:** stateless, subnet rules for inbound and outbound
- **Security Groups:** stateful, operate at the EC2 instance level or ENI (stateful means if you send a request from your instance, the response traffic for that request is allowed to flow in regardless of inbound security group rules. Responses to allowed inbound traffic are allowed to flow out, regardless of outbound rules)
- **VPC Peering:** connect two VPC with non overlapping IP ranges, non transitive
- **VPC Endpoints:** provide private access to AWS Services within VPC
- **VPC Flow Logs:** network traffic logs
- **Site to Site VPN:** VPN over the public internet between on premise data center and AWS
- **Direct Connection:** direct private connection to AWS

# Simple Storage Service (AWS S3)

- Main building block of AWS
- Advertised as "infinetly scaling" storage

## S3 Buckets and Objects

- S3 allows people to store **objects** (files) in **buckets** (directories)
- Buckets must have **globally unique name**
- Buckets are **defined at the region level**
- Naming convention
  - No uppercase
  - No underscore
  - 3-63 characters long
  - Not an IP
  - Must start with lowercase letter or number

### S3 Overview -- Objects

- Objects (files) have a key
- The **key** is the **FULL** path:
  - s3://my-bucket/**my_file.txt**
  - s3://my-bucket/**my_folder/another_folder/my_file.txt**
- The key is composed of **prefix** + **object name**
  - So, from above, **my_folder/another_folder/** would be the **prefix** and **my_file.txt** would be the object name
- Max object size if 5TB (5000GB)
- If uploading more than 5GB, you must use **multi-part upload**
- Each object can have **metadata** (list of text key/value pair)
- Can also have Tags (Unicode key/value pair -- up to `10`)
- Object can also have Version ID (if versioning is enabled)

## S3 Versioning

- Not enabled by defauld
- It is enabled at the **bucket level**
- Same key overwrite will increment the version
- Best practive is to version your buckets
  - Protects against unintended deletes (i.e., ability to restore a version)
  - Easily roll back to a previous version
- Notes:
  - Any file that is not versioned prior to enabling versioning will have a version of **null**
  - Suspending versioning does not delete the previous versions

## S3 Encryption for Objects

- There are `4` methods of encrypting objects in S3:
  - `SSE-S3`: encrypts S3 objects using keys handled and managed by AWS
  - `SSE-KMS`: leverages AWS Key Management Service to manage encryption keys
  - `SSE-C`: when you want to manage your own encryption keys
  - Client Side Encryption

### `SSE-S3`

- `SSE-S3`: encryption using keys handled and managed by AWS
- Object is **encrypted server side**
- `AES-256` encryption
- Must set header: `"x-amx-server-side-encryption":"AES256"`

![](https://s3.amazonaws.com/gutucristian.com/SSE-S3.png)

### `SSE-KMS`

- `SSE-KMS`: encryption using keys handled and managed by AWS
- KMS advantages: user control + audit trail
- Object is **encrypted server side**
- Must set header: `"x-amx-server-side-encryption":"aws:kms"`

![](https://s3.amazonaws.com/gutucristian.com/SSE-KMS.png)

### `SSE-C`

- `SSE-C`: **server side encryption** using data keys fully managed by the customer outside of AWS
- Amazon S3 **does not store the encryption key** you provide
- **`HTTPS` must be used** (because you are sending a secret over the wire to AWS)
- Encryption key must be provided in `HTTP` headers for every `HTTP` request made

![](https://s3.amazonaws.com/gutucristian.com/SSE-C.png)

### Client Side Encryption

- Data is encrypted before sending it over to S3
- Client must decrypt the data themselves when retrieving from S3
- Customer fully manages encryption / decryption cycle

![](https://s3.amazonaws.com/gutucristian.com/CSE.png)

### Encryption in transit

- In terms of retrieving an object S3 exposes:
  - `HTTP` endpoint: non encrypted
  - `HTTPS` endpoint: encryption in flight
- Note: `HTTPS` is **mandatory** for `SSE-C`

## S3 Security

- **User based**
  - IAM policies -- which API calls should be allowed for a specific user from IAM console
- **Resource based**
  - Bucket Policies -- bucket wide rules that we can set from the S3 console (bucket rules say what **principals** can and cannot do on our S3 bucket -- enables us to do cross account access on our buckets)
  - Object Access Control List (ACL) -- finer grain (we set the access rule at the **object level**)
  - Bucket Access Control List (ACL) -- less common
- **Note:** an IAM principal can access an S3 object if:
  - The user IAM permissions allow it **OR** the bucket policy **ALLOWS** it
  - **AND** if there is no explicit **DENY**
  - Thus: explicit **DENY** in an IAM policy will take precedence over a bucket policy permission

## Bucket Policies

- JSON based policies:
  - `Resource`: buckets and object which policy applies to
  - `Action`: Set of actions to ALLOW or DENY (as dictated by `Effect`)
  - `Effect`: ALLOW or DENY actions defined in `Action` array
  - `Principal`: the account of user to apply the policy to
- Bucket policies are also commonly used to:
  - Grant public access to a bucket
  - Force objects to be encrypted at upload
  - Grant access to another account (Cross Account S3 Bucket policies)

## Bucket settings for Block Public Access

- Below details are not required for the exam, but, in general, you should know that it is possible to block public access to your S3 bucket through these settings
- Block public access to buckets and objects granted through:
  - *new* access control lists (ACLs)
  - *any* access control lists (ACLs)
  - *new* public bucket or access point policies
- Block public and cross-account access to buckets and objects through any public bucket or access point policies

## S3 Security -- Other

- Networking:
  - Can access S3 privately through VPC Endpoints (e.g., an EC2 instance in a VPC can use VPC Endpoints to make a request to S3 without going through public internet)
- Logging and Audit:
  - S3 Access Logs can be stored in other S3 buckets
  - API calls can be logged in AWS CloudTrail
- User Security:
  - MFA Delete: Multi Factor Authentication can be setup so that it is required in versioned objects whenever someone tries to delete objects
  - Pre-Signed URLs: URLs that are valid only for a limited time (e.g., premium video service)

## S3 Websites

- S3 can host static websites and have them accessible on the public internet
- The website URL will look like: `<bucket-name>.s3-website-<AWS-region>.amazonaws.com`
- If you get a `403 Forbidden` error, make sure the bucket policy allows public reads

## Cross-Origin Resource Sharing (CORS) Explained

- An **origin** is a scheme (protocol) + host (domain) + port (e.g., `https://www.mydomain.com:8080`)
- (CORS) is an HTTP-header based mechanism that allows a server to indicate any other origins (domain, scheme, or port) than its own from which a browser should permit loading of resources
- Read more [here](https://medium.com/@electra_chong/what-is-cors-what-is-it-used-for-308cafa4df1a)
- Preflight Request: a request that browser does before the actual request is sent to check that the cross origin allows us to make a request to it
- Preflight Response: returns back to us information regarding whether we are allows to perform the cross origin request to the specified endpoint along with all of the allowed methods

![]()

## S3 CORS

- If a client does a cross origin request on our S3 bucket, we need to enable the correct CORS headers
- You can allow for a specific origin (or `*` for all origins)

![]()

## S3 Consistency Model -- Before December 2020

**Before December 2020 S3 did not have strong eventual consistency**

There was a happy path scenario. This is where we are writing a brand new object to an S3 bucket.

1. `PUT /key-prefix/cool-file.jpg 200`
2. `GET /key-prefix/cool-file.jpg 200`

When we `GET` the file right after we `PUT` we get a status code of `200` and we know the file is the most up to date copy. Otherwise known as **read-after-write** consistency

But then there was this caveat scenario with overwriting. We write an object to the bucket. Another process writes that object again (with new content), and then we try to read the object:

1. `PUT /key-prefix/cool-file.jpg 200`
2. `PUT /key-prefix/cool-file.jpg 200` (new content)
3. `GET /key-prefix/cool-file.jpg 200`

Here we **used to** end up with the **eventual consistency** issue. Namely, when we called `GET` we could have received the file contents of the first `PUT` or the second. The reason being that S3 is a distributed service and it could take time for the change to be propagated behind the scenes -- thus yielding the the **eventual consistency** problem.

Then there was the fussy `404` caveat. This occurred when you would issue a `GET` before the `PUT` had finished.

1. `GET /key-prefix/cool-file.jpg 404`
2. `PUT /key-prefix/cool-file.jpg 200`
3. `GET /key-prefix/cool-file.jpg 404`

Here because the `GET` happened on the object before the `PUT` was complete, we got a `404`. Because of **eventual consistency**, it was possible to get a `404` again (after `2`) because the `PUT` may have still been propagating.

## S3 Consistency Model -- Post December 2020 (S3 Strong Consistency)

AWS announced strong **read-after-write consistency** for all `GET`, `PUT`, and `LIST` operations in S3.

So what does that mean for the scenarios we talked about up above? Well, if a `PUT` call of an object to S3 is successful you can assume that **any** subsequent `GET` or `LIST` call for that object will return the latest version of the object.

The overwrite scenario will return the latest data as well. **But the first GET request must happen after all PUT requests have finished to guarantee the latest object.**

This last bit is important. You can have concurrent processes writing the same object, with different data, to the same bucket:

1. The first process finishes writing the object and then the next process starts writing the object with new data
2. Meanwhile, **before this second write finishes**, we start a `GET` request on the object

In this scenario our `GET` request will return the object with data before the second `PUT` and, as expected, this is because the second `PUT` hasn't yet completed.

We can also have a scenario where there are **simultaneous writes**. Meaning that before process one finishes writing the object, process two starts writing to that object as well. This is what we call **concurrent writes**. In this scenario, S3 uses **last-write wins semantics**. But our GET request will return **mixed results until the final write finishes.**

Basically the issue before was that even after a successful `PUT` S3 did not guaranteed that a subsequent `GET` request for this object will return the latest object that **we just wrote** because of eventual consistency and the time required to propagate changes across entire system. With the new update there is **strong read-after-write consistency** which means that after a successful `PUT` request any subsequent `GET` is guaranteed to get the data that was just written (not an older previous version).

Read more from [here](https://blog.kylegalbraith.com/2021/01/12/the-s3-consistency-model-got-an-upgrade/).

# AWS CLI

- Developing and performing AWS tasks against AWS can be done in several ways:
  - Using the AWS CLI on our local computer (Create Access keys and use them to set up in AWS CLI `aws configure`)
  - Using the AWS CLI on our EC2 machines
  - Using the AWS SDK on our local machine
  - Using the AWS SDK on our EC2 machines
  - Using the AWS Instance Metadata Service for EC2
- Note: to create additional AWS CLI profiles run `aws configure --profile <your-desired-profile-name>`

## AWS CLI on EC2

- The **BAD** way:
  - Run `aws configure` as we do locally on the EC2 instance
  - This is dangerous because if EC2 instance gets compromised then so will your account
- The **RIGHT** way:
  - Use an IAM Role and attach it to the EC2 instance (Note: an EC2 instance can only have up to one role at a time)
  - IAM Roles can come with a policy authorizing exactly what the EC2 instance should be able to do
  - Anytime your EC2 instance needs to perform something, never put your credentials on it, always use an IAM Role

![]()

## IAM Policy Simulator

Among other things, can be used to test the policies with selected services, actions, and resources. For example, you can test to ensure that your policy allows an entity to perform the ListAllMyBuckets, CreateBucket, and DeleteBucket actions in the Amazon S3 service on a specific bucket.

Find it [here](https://policysim.aws.amazon.com/home/index.jsp?#).

## AWS CLI Dry Runs

Sometimes we just want to make sure we have the permissions to run a command, but not actually run it. 

For example, we want to test that we can use the AWS CLI to create an EC2 instance, but not actually create it because it may be expensive.

For this some AWS CLI command have the `--dry-run` option to simulate API calls.

## AWS CLI STS Decode Errors

- When you run API calls and they fail you can get long error messages
- Decodes additional information about the authorization status of a request from an encoded message returned in response to an AWS request: `aws decode-authorization-message --encoded-message <encoded-err-msg-value>`

## AWS EC2 Instance Metadata

- AWS EC2 Instances Metadata allows EC2 instances to learn about themselves **without using an IAM Role for that purpose**
- The URL is http://169.254.169.254/latest/metadata
- This IP is an internal IP to AWS (it will not work from your computer) it will only work from the EC2 instance (Note: you cannot retrieve the AIM Policy from the instance metadata URL)
- Metadata = info about the EC2 instance
- Userdata = launch script of the EC2 instance
- Good to know:
  - You can retrieve the role name attached to your EC2 instance using the metadata service but not the policy itself
  - You can also retrieve the temporary security credentials that the EC2 instance is using to make API calls based on its attached IAM Role through the instance metadata endpoint

## MFA with CLI

To use MFA with the CLI you must create a temporary session.

To do so, you must run the **STS GetSessionToken** API call:

`aws sts get-session-token --serial-number arn-of-the-mfa-device --token-code code-from-token --duration-seconds 3600`

Which would return something like:

![]()

That has a new access key and id a session token (which we will need to use in all the API calls to AWS) and an expiration date time.

## AWS SDK Overview

- Use if you want to perform actions on AWS without directly going through AWS Console or AWS CLI
- Good to know: if you don't specify or configure a default region, then `us-east-1` is chosen by default by the SDK

## AWS Limit (Quotas)

- API Rate Limits (how many times you can call an API in a row)
  - **DescribeInstances** API for EC2 has a limit of `100` calls per second
  - **GetObject** on S3 has a limit of `5500` GET per second per prefix
  - For intermitent errors: implement exponential backoff strategy
  - For consistent errors: request an API throttling limit increase
- Service Quotas (service limits)
  - Running on-demand standard instances: `1152` vCPU
  - You can request a service limit increase by **opening a ticket**
  - You can request a service quota increase by using the **Service Quotas API**

### Exponential Backoff

- If you get a **ThrottlingException** intermittently, use exponential backoff
- Retry mechanism included in SDK API calls
- Must implement yourself if using the API as is w/o SDK
- Exponential backoff means that everytime an API call fails with double the amount of time we wait before making another request

## AWS Credentials Provider & Chain

**When using the CLI** it will look for credentials in this order:

1. **Command line options**: `--region`, `--output`, `--profile`
2. **Environment variables:** `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_SESSION_TOKEN`
3. **CLI credentials file**: `~/.aws/credentials` on Mac
4. **CLI configuration file:** `~/.aws/config` on Mac
5. **Container credentials:** for ECS tasks
6. **Instance profile credentials:** for EC2 Instance Profiles

**When using an SDK** (e.g., the Java SDK) it will look for credentials in this order:

1. **Environment variables:** `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_SESSION_TOKEN`
2. **Java system properties:** `aws.accessKeyId` and `aws.secretKey`
3. **The default credential profiles file:** `~/.aws/credentials`
4. **Amazon ECS container credentials** for ECS containers
5. **Instance profile credentials** for EC2 instances

## AWS Credentials Scenario

Assume we have a scenario where an application deployed to an EC2 instance has Access Key and Id credentials from an IAM user set in the environment variables which are being used to make API calls to S3. Furthermore, assume that the IAM user has `S3FullAccess` permissions.

As a developer, you realize this is against best practices and decide to:

1. Create a separate IAM Role and EC2 Instance Profile for the EC2 instance, and
2. Assign the role (which has minimum read only permissions to the specific bucket that it needs) to the EC2 instance

Out expectation is that now the EC2 instance will use the instance profile (which defines *who am I*) to assume the role it was assigned and because of that will noe only have access read only permissions to the specific S3 bucket that we defined.

However, you notice that despite this the EC2 instance **still** has access to all S3 buckets. Why?

**Answer:** the credentials chain is still giving priority to the environment variables. So, to get the behaviour we desire, we would have to delete the environment variables from the EC2 instance and, after that, step `5` in the chain will become most relevant and credentials will be retrieved from the **instance profile**.

Read [here](The difference between an AWS role and an instance profile) on more about AWS Role vs instance profile.

## AWS Credentials Best Practices

- **Never ever** store AWS credentials in your code
- Best practice is for your credentials to be inherited from the credentials chain
- So, **if working within AWS, use IAM Roles**:
  - EC2 Instance Roles + EC2 Instance Profile for EC2 instances
  - ECS Roles for ECS tasks
  - Lambda Roles for Lambda functions
  - Ultimatelly, the idea is that we assign a role to each service and the service assumes the role with temporary credentials and is able to perform API calls depending on what the role defines (read more [here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)
- If working **outside of AWS** use environmental variables / named profiles

## Signing AWS API requests

- When you call the AWS HTTPS API you sign the request (so that AWS can identify you) using your AWS credentials (access key & secret key)
- **Note:** some requests to Amazon S3 don't need to be signed
- If you use the SDK or CLI, the HTTP requests are already signed **for you**
- As an AWS Certified Developer you only need to know at a *high level* how AWS HTTP requests are signed using Signature v4 (SigV4) -- an AWS protocol

![]()

### SigV4 Requests Examples

HTTP Header Option:

![]()

Query String Option (e.g., S3 pre-signed URLs)

![]()

# Advanced S3 & Athena

## S3 MFA Delete

- MFA (multi factor authentication) forces user to generate a code on a device (usually mobile) before doing **important** operations on S3
- To use MFA Delete versioning must be enabled on the S3 bucket
- You will need MFA to:
  - Permanently delete an object version
  - Suspernd versioning on the bucket
- You won't need MFA for:
  - Enabling versions
  - Listing deleted versions, etc..
- Only the bucket owner (root account) can enable/disable MFA Delete
- MFA Delete currently can only be enabled using the CLI

## S3 Access Logs

- For audit purposes we may want to log all access to S3 buckets
- We can enable so that any request made to some S3 bucket(s), from any account, authorized or denied, will be logged into another S3 bucket so we can analyze it later (using AWS Athena for example)
- **Note:** never set the logging bucket to be the bucket that you are monitoring -- if you do this, then it will create a logging loop and your bucket will grow infinetly

## S3 Replication -- Cross Region Replication (CRR) and Same Region Replication (SRR)

- In order for CRR or SRR to work you **must enable versioning in source and destination buckets**
- Buckets can be in different accounts
- Copying is asynchronous
- Must give proper IAM permissions to S3 (so that first bucket has permissions to copy to second bucket)
- CRR use cases: compliance, lower latency access across regions, replication across accounts
- SRR use cases: log aggregation, live replication between production and test accounts

### S3 Replication -- Notes

- **After activating only new objects are replicated** (so its not retroactive)
- `DELETE` operations are not replicated
- There is no "chaining" of replication:
  - If bucket `1` has replication into bucket `2` and bucket `2` has replication into bucket `3`, when we write to bucket `1` it does not replicate into bucket `3` (so not transitive)

## S3 Pre-Signed URLs

- Can generate pre-signed URLs using SDK or CLI
  - For uploads (easy, can use CLI)
  - For downloads (harder, must use the SDK)
- Valid for a default of `3600` seconds -- can change timeour with `--expires-in <time-by-seconds>` argument
- Users given a pre-signed URL inherit the permissions of the person who generated the URL for `GET` / `PUT`
- Example use cases:
  - Allow only logged-in users to download a premium video on your S3 bucket
  - Allow an ever changing list of users to download files by generating URLs dynamically
  - Allow a user to upload a file to a precise location temporarily (e.g., a profile picture)

 ## S3 Storage Classes
 
 - Standard -- General Purpose
 - Standard-Infrequent Access (IA)
 - One Zone-Infrequent Access
 - Intelligent Tiering
 - Glacier
 - Glacier Deep Archive

### Standard -- General Purpose

- High durability of objects across multiple AZs
- Can sustain `2` concurrent facility failures
- Use cases: mobile & gaming applications, big data analytics, content distribution

### Standard-Infrequent Access (IA)

- High durability of objects across multiple AZs
- Suitable for data that is less frequently accessed (but required rapid access when needed)
- Slightly lower cost than S3 Standard tier
- Can sustain `2` concurrent facility failures
- Use cases: data store for disaster recovery, backups, etc..

 ### One Zone-Infrequent Access
 
 - Same as IA tier but data is stored in a **single AZ** -- so decreased availability (will be lost if AZ is destroyed)
 - Cost is 20% lower than IA tier
 - Use cases: storing secondary backups copies of on-premise data or data that we can re-create

### Intelligent Tiering

- High durability of objects across multiple AZs
- Same low latency and high throughput performance of S3 standard
- Small monthly monitoring and auto-tiering fee
- Automatically moves objects between two access tiers based on changing access patterns

### Glacier

- Low cost object storage meant for archiving / backup
- Data is retained long term (`10`s of years)
- Alternative to on-premise magnetic tape storage
- High durability
- Storage cost per month is $0.004 / GB + retrieval cost
- Each item in Glacier is not called an object, but, instead, an **archive** and can be up to `40` TB

### Glacier & Glacier Deep Archive

- Amazon Glacier - `3` retrieval options:
  - Expedited (`1` to `5` minutes)
  - Standard (`3` to `5` hours)
  - Bulk (`5` to `12` hours)
  - Minimum storage duration of `90` days
- Amazon Glacier Deep Archive -- for long term storage (cheaper):
  - Standard (`12` hours)
  - Bulk (`48` hours)
  - Minimum storage duration of `180` days

### S3 Storage Tiers Summary

![]()

## S3 Lifecycle Rules

- You can transition objects between storage classes
- For infrequent accessed objects, move them to `STANDARD_IA`
- For archived objects that we don't need in real time move to `GLACIER` or `DEEP_ARCHIVE`
- Moving objects can be **automated** using a **lifecycle configuration**

![]()

- **Transition actions:** it defines when objects are transitioned to another storage. For example:
  - Move objects to Standard IA class `60` days after creation
  - Move to Glacier class `6` months after that
- **Expiration actions:** configure objects to expire (delete) after some time. For example:
  - Access log files can be set to be deleted after `N` days
  - **Can be used to delete old versions of files (if versioning is enabled)** -- for example, delete old versions of files after `60` days
  - Can be used to delete incomplete multi-part uploads
- Rules can be created for a certain prefix (e.g., prefix of `s3://mybucket/mp3/*`)
- Rules can be created for certain object tags (e.g., `Department: Finance`)

## S3 Baseline Performance

- S3 automatically scales to high request rates (latency `100` - `200` ms)
- Your application can achieve at least `3500 PUT/COPY/POST/DELETE` and `5500 GET/HEAD` requests per second per prefix in a bucket

### S3 KMS Limitation

- If you use `SSE-KMS` you may be impacted by the KMS limits
- When you **upload** it calls the `GenerateDataKey` KMS API and when you **download** it calls the `Decrypt` KMS API
- The KMS quota for requests per second for these operations is either `5500`, `10000`, or `30000` depending on the region
- As of today you cannot request a quota increase for KMS

### S3 Increasing Upload Performance

- **Multi-Part upload:**
  - Recommended for files greater than `100` MB and must use for files greater than `5` GB
  - The idea of multi-part upload is that we split file into smaller pieces which allows us to upload in parallel -- this speeds up transfers

![]()

- **S3 Transfer Acceleration (upload only):**
  - Increase transfer speed by transferring file to an AWS edge location (via public internet) which will forward the data to the S3 bucket in the target region (via private AWS network -- which is faster)
  - Compatible with multi-part upload
  - The idea is that we minimize the amt. of public internet which we go through (which is slower) and maximize the amt. of private AWS internet that we go through (which is faster)

![]()

### S3 Increasing Download Performance

Use S3 **Byte-Range Fetches** to parallelize `GET`s by requesting specific range of bytes in our file -- we can pick and choose what sections we are downloading.

![]()

## S3 Select & Glacier Select

- Retrieve less data using SQL by performing **server side filtering**
- Can filter S3 data by rows and columns (simple SQL statements only -- no aggregations, etc..)
- Less network transfer, less CPU cost client-side
- For more complex querying we can use AWS Athena

![]()

## S3 Event Notifications

- Trigger certain actions (to three possible targets: SNS, SQS, or Lambda) when there is a state change on some S3 bucket (e.g., `S3:ObjectCreated`, `S3:ObjectRemoved`, `S3:Replication`, etc..)
- Object name filtering possible (e.g., `\*.jpg`)
- Classic use case: generate thumbnails of images uploaded to S3
- S3 event notifications typically take a few seconds to be delivered but can sometimes take a minute or longer
- If two writes are made to a single non-versioned bucket at the same time, it is possible that only a single event notification will be sent
- If you want to ensure that an event notification is sent for every successful write, you must enable versioning on your bucket

![]()

## AWS Athena

- **Serverless** service to perform analytics **directly against S3 files**
- Uses SQL to query the files
- Has a JDBC / ODBC driver
- Charged per query and amount of data scanned
- Supports CSV, JSON, ORC, Avro, and Parquet (built on Presto)
- Can be used to query VPC Flow Logs, ELB Logs, CloudTrail trails, build BI / analytics / reporting tools, etc..

## S3 Object Lock & Glacier Vault Lock

- **S3 Object Lock**
  - Adopt a WORM (Write Once Read Many) model
  - Block an object version for a specified amount of time
- **Glacier Vault Lock**
  - Adopt a WORM (Write Once Read Many) model
  - Lock the policy for future edits (can no longer be changed)
  - Helpful for compliance and data retention

# AWS CloudFront

## CloudFront Overview

- Content Delivery Network (CDN)
- Improves read performance by caching content at **Edge Locations** (`216` globally)
- Provides DDoS protection, integration with Shield, AWS Web Application Firewall (WAF)
- Can expose external HTTPS and can talk with internal HTTPS backend

### CloudFront Origins

- **S3 bucket**
  - For distributing files and caching them at the edge
  - Enhanced security with CloudFront Origin Access Identity (OAI)
  - CloudFront can be used as an ingress (to upload files to S3)
- **Custom Origin (HTTP)**
  - Application Load Balancer
  - EC2 Instance
  - S3 Website (must first enable the bucket as a static website)
  - Any HTTP backend you want

![]()

### CloudFront S3 as an Origin

![]()

### ALB or EC2 as an Origin

![]()

### Geo Restriction

- We can **whitelist** and **blacklist** certain countries in CloudFront based on business requirements
- Country is determined by a `3rd` party Geo-IP database

### CloudFront vs S3 Cross Region Replication

- CloudFront:
  - **Global Edge Network**
  - Files are **cached** for some predefined TTL
  - Good for **static content** that must be **available everywhere**
- S3 Cross Region Replication:
  - Must be setup for **each region** you want replication to happen in (i.e., not global)
  - Files are updated in **near real time**
  - Read only

## CloudFront Caching

- Cache cann be based on:
  - Headers
  - Session Cookies
  - Query String Parameters
- The cache lives at each CloudFront **Edge Location**
- Our goal is to maximize the amount of cache hits and minimize requests to origin
- We can control the cache TTL (`0` seconds to `1` year) -- which can be set by the origin using `Cache-Control` header, `Expires` header...
- We can also invalidate part of the cache using the `CreateInvalidation` API

### Maximize cache hits by separating **static** and **dynamic** distributions

![]()

## Cloud Front Security

- As mentioned in Geo Restriction, we can **whitelist** and **blacklist** certain countries in CloudFront based on business requirements
- **Viewer Protocol Policy:**
  - Redirect HTTP to HTTPS
  - Or use HTTPS only
- **Origin Protocol Policy (HTTP or S3):**
  - You can set it to be HTTPS, or
  - You can set it to Match Viewer policy which means that if request came in as HTTP (or HTTPS) it will go to origin also as HTTP (or HTTPS)
- **Note:** S3 bucket websites do not support HTTPS

## CloudFront Signed URL / Signed Cookie

- Say you want to distribute paid shared content to premium users over the world
- We can use CloudFront Signed URL / Cookie and need to attach a policy that defines:
  - URL expiration
  - Allowed IP ranges to access the data from
  - Trusted signers (which AWS accounts can create signed URLs)
- Signed URL = access to individual files (one signed URL per file)
- Signed Cookies = access to multiple files (one signed cookie for many files)

### CloudFront Signed URL Diagram

![]()

### CloudFront Signed URL vs S3 Pre-Signed URL

At a high level, use S3 Pre-Signed URL if you want to distribute file securely to user w/o having CloudFront layer (so going to S3 directly), else use CloudFront Signed URL.

![]()

# CloudFront

## Docker

- Apps are packaged into **containers** that can be run ony OS
- Apps run the same regardless of where they run
- In Docker, resources are shared with the host and we can have many containers on one server

![]()

### Docker Image

- A docker image is a set of **layers** that describe a container
- **A running instance of an image is a container**
- Docker images are stored in Docker repositories (e.g., DockerHub, AWS Elastic Container Registry)

### Docker Container Management

- To manage containers we need a container management platform
- In AWS we have three choices:
  - AWS Elastic Container Service (ECS)
  - Fargate (Amazon's own serverless container management platform)
  - EKS (Amazon's managed Kubernetes)

## ECS Clusters

- ECS Clusters are a logical grouping of EC2 instances
- ECS instances run the ECS agent (Docker containers)
- An instance of the ECS agents runs on each EC2 instance in the ECS cluster and (among other things) is responsible for registering the container instances to the ECS cluster (read more [here](https://www.allthingsdistributed.com/2015/07/under-the-hood-of-the-amazon-ec2-container-service.html))
- The EC2 instances run a special AMI made specifically for ECS

### Bootstrapping Container Instances with Amazon EC2 User Data

When you launch an Amazon ECS container instance, you have the option of passing user data to the instance. The data can be used to perform common automated configuration tasks and even run scripts when the instance boots. For Amazon ECS, the most common use cases for user data are to pass configuration information to the Docker daemon and the Amazon ECS container agent.

The Linux variants of the Amazon ECS-optimized AMI look for agent configuration data in the `/etc/ecs/ecs.config` file when the container agent starts. You can specify this configuration data at launch with Amazon EC2 user data.

To set only a single agent configuration variable, such as the cluster name, use echo to copy the variable to the configuration file:

{% highlight bash linenos %}
  #!/bin/bash
  echo "ECS_CLUSTER=MyCluster" >> /etc/ecs/ecs.config
{% endhighlight %}

## ECS Task Definition

A task definition is metadata in JSON form that tells ECS how to run a Docker container in our cluster.

It contains important information like:
 - Image Name
 - Port Binding for Container and Host
 - Memory and CPU required
 - Environment variables
 - Networking information

## ECS Task

A Task is created when you run a Task directly, which launches container(s) (defined in the task definition) until they are stopped or exit on their own, at which point they are not replaced automatically. Running Tasks directly is ideal for short running jobs, perhaps as an example things that were accomplished via CRON.

## ECS Service

A Service is used to guarantee that you always have some number of Tasks running at all times. If a Task's container exits due to error, or the underlying EC2 instance fails and is replaced, the ECS Service will replace the failed Task. This is why we create Clusters so that the Service has plenty of resources in terms of CPU, Memory and Network ports to use. To us it doesn't really matter which instance Tasks run on so long as they run. A Service configuration references a Task definition. A Service is responsible for creating Tasks.

## ECS Service with Load Balancer

- This time we don't specify a host port. Instead, we just specify a container port and the host port will be random
- The load balancer (only works with ALB and NLB not CLB) uses **dynamic port forwarding** to spread the load dynamically across the containers

![]()

## ECR

- ECR is a private Docker image repository
- Access is controlled through IAM (if we have permissions error, then it is an IAM policy issue)
- Docker Push & Pull from ECR:
  - `docker push <full-ecr-image-url>`
  - `docker pull <full-ecr-image-url>`
- AWS CLI ECR login commands:

![]()

## Fargate

- Before, when we launched an ECS Cluster we had to create our EC2 instances and if we needed to scale we had to add more EC2 instances
- So we managed the infrastructure..
- With Fargate, it's all **serverless**
- We don't have to provision EC2 instances
- We just create task definitions and AWS will run the containers for us
- To scale we just increase the task number -- we no longer have to worry about the underlying infrastructure as we did with EC2

## ECS IAM Roles

- **EC2 Instance Profile:**
 - An instance profile defines “who am I?” Just like an IAM user represents a person, an instance profile represents EC2 instances. The only permissions an EC2 instance profile has is the power to **assume a role** (read more [here](https://medium.com/devops-dudes/the-difference-between-an-aws-role-and-an-instance-profile-ae81abd700d))
  - Used by the ECS Agent to make API calls to ECS service, send container logs to CloudWatch, to pull Docker images from ECR, etc..
- **ECS Task Role:**
  - Allow each task to have a specific role
  - Use different task roles for different ECS service you run
  - Task roles are defined in the **task definition**
- Important parts here:
  - We want to have an IAM role at the EC2 instance level to help the ECS Agent
  - We want task roles at the task level, so that each task has just enough permissions but not more than it needs

![]()

For the exam you need to remember:

1. Your ECS instance role that gets attached to your EC2 instance allows your ECS agent to function correctly
2. For each task that we create we should create a separate IAM Task Role

## Task 

- When a task of type EC2 is launched, ECS must determine where to place it with the constraints of **CPU, memory,** and **available port**
- Similarly, when a service scales in, ECS needs to determine which tasks to terminate
- To assist with this, you can define a **task placement strategy** and **task placement constraints**
- Note this only applies to ECS with ECS not for Fargate -- Fargate does all of this for us since its serverless and we don't manage any infrastructure

![]()

## ECS Task Placement Process

Task placement strategies are a best effort.

When AWS ECS places tasks it uses the following process to select container instances:

1. Identify the instance that satisfy the CPU, memory, and port requirement in the task definition (so basically which instances can we even place the task on in the first place)
2. Identify the instances that satisfy the task placement **constraints**
3. Identify the instances that satisfy the task placement **strategy**
4. Select the instance for task placement

## ECS Task Placement Strategies

- **Binpack:**
  - Place tasks such that we use the amount of resource (i.e., tries to pack all Tasks in an existing instance to fill it up before putting in a new one)
  - This minimizes number of instance and, thus, cost
- **Random:**
  - Places the tasks randomly
- **Spread:**
  - Spreads the tasks evenly based on the specified **value**
  - This value can be an ECS Availability Zone, instance id, etc..

Note that we can mix task placement strategies.

## ECS Task Placement Constraints

- **distinctInstance:** place each task on a different container instance
- **memberOf:** places task on instances that satisfy the expression (e.g., place task only on instances that are of type `t2.*`)
  - Uses the **Cluster Query Language**

## ECS -- Service Auto Scaling

- CPU and RAM are tracked in CloudWatch at the ECS service level
- **Target Tracking:** target a specific average CloudWatch metric
- **Step Scaling:** scale based on CloudWatch alarms
- **Scheduled Scaling:** scale based on a schedule b/c of predictable changes
- Note: ECS Service Scaling (task level) is not the same as EC2 Auto Scaling (instance level). In other words, if the serice scaling sees that it needs to scale the task count up (to meet some new demand) but we don't have enough underlying EC2 resources in our cluster to handle this, then we won't be able to increase the task count. This is because the the number of EC2 instances **will not**, by default, scale up
- With Fargate Auto Scaling, the issue of underlying hardware not being able to support scaling no longer is a problem because Fargate is serverless and it manages all the infrastructure for us

## ECS -- Cluster Capacity Provider

- A Capacity Provider is used in association with a cluster to determine the infrastructure that a task runs on
  - For ECS with Fargate, the FARGATE and FARGATE_SPOT capacity providers are added automatically
  - For ECS on EC2, you need to associate the capacity provider with an auto scaling group (this will allow your auto scaling group to automatically add EC2 instances when needed)
- When you run a task or service you define a capacity provider strategy to prioritize in which provider to run
- This allows the capacity provider to automatically provision infrastructure for you

![]()

## ECS Summary and Exam Tips

- ECS is used to run Docker containers and has three flavors
- ECS "Classic": provision EC2 instances to run containers onto
- Fargate: ECS Serverless (removes need to provision and manage EC2 infrastructure)
- EKS: Managed Kubernetes by AWS

### ECS Classic

- EC2 instances must be created
- We must configure the file `/etc/ecs/ecs.config` with the cluster name so that ECS agent knows what cluster instance is part of
- The EC2 must run an ECS agent (which allows us to register an EC2 instance with the cluster)
- The EC2 instance can run multiple containers of the same type:
  - You must **not** specify a *host port* (only *container port*)
  - You should use an **Application Load Balancer** with the **dynamic port mapping**
  - The EC2 instance security group must allow traffic from the ALB on all ports for dynamic port forwarding to work
- ECS tasks must have IAM Roles to execute actions against AWS
- Security groups operate at the **instance level** not the task level

### ECR is used to store Docker Images

- ECR is tightly integrated with IAM
- AWS CLI v1 login command: `$(aws ecr get-login --no-include-email --region us-east-1)`
- AWS CLI v2 login command: `aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin MY-REGISTRY-URL`
- Docker `push` and `pull`:
  - `docker push MY-REGISTRY-URL`
  - `docker pull MY-REGISTRY-URL`
- In case an EC2 instance (or you) cannot pull a Docker image check IAM

### Fargate

- Fargate is serverless (no EC2 to manage)
- AWS provisions containers for us and assigns ENI
- Fargate containers are provisioned based on the container spec (task definition)
- Fargate tasks must have corresponding IAM Roles in order to execute actions against AWS

### ECS Other

- ECS does integrate with CloudWatch Logs:
  - You need to setup logging at the task definition level
  - Each container will have a different log stream
  - The EC2 Instance Profile needs to have the correct IAM permissions
- Use IAM Task Roles for your tasks
- Task Placement Strategies: binpack, random, spread
- **Service Auto Scaling** with target tracking, step scaling, or scheduled
- **Cluster Auto Scaling** can be achieved through Capacity Provider by associating it with an Auto Scaling Group

# Elastic Bean Stalk

- AWS Elastic Beanstalk is an easy-to-use **service for deploying and scaling web applications and services**
- There is no additional charge for Elastic Beanstalk - you pay only for the AWS resources needed to store and run your applications
- Three popular architecture models used with Elastic Beanstalk:
  - Single instance deployment (good for development environments)
  - Load Balancer + Auto Scaling Group (good for production or pre-production web applications)
  - ASG only (great for non-web apps in production -- e.g., workers, etc..)
- Elastic Beanstalk has three components:
  - Application
  - Application version (each deployment gets assigned a version)
  - Environment name (can be whatever you want -- e.g., dev, test, prod)
- You deploy application versions to environments and can **promote** them to the next environment
- Has feature to rollback to previous application version

![]()

- If you create an RDS instance with Beanstalk, then if you delete your RDS environment your RDS instance will also be deleted (if you want RDS to be decoupled from EB, then create a separate RDS instance outside of Beanstalk and point to it instead)
- You cannot change load balancer type after creating an EB environment -- if you want to change it you will have to create another environment
- With EB we can configure instances to log to S3, CloudWatch
- We can also configure X-Ray to trace all infrastructure communications
- We can pass environment variables to instances
- Regarding instances, we can specify root volume type, security groups
- In terms of capacity, we can define an ASG for out instances, EC2 fleet composition (e.g., on demand, spot instances, etc..), AMI ID, instance type, number of AZs to launch in, AZ placement, scaling policies / triggers
- We can define which load balancer we would like to use (e.g., ALB, CLB, NLB) as well as security groups for load balancers
- For the load balancers we can configure settings so that it's logs go into a dedicated S3 bucket
- We also need to define how Elastic Beanstalk should deploy our infrastructure (there are various deployment modes)
- We can define databases EB should launch for us

## Elastic Beanstalk Deployment Modes

![]

1. **All at once (deploy all in one go):** fastest, but instances aren't available to serve traffic for a bit (downtime)
2. **Rolling:** update a few instances at a time (bucket) and then move onto the next bucket once the first bucket is healty (for a short period of time, the application will be operating below full capacity)
3. **Rolling with additional batches:** like rolling, but spins up new instances to move the batch (so that old application is still available and operating at full capacity)
4. **Immutable:** spins up new instances in a completely new ASG, deploys version to these instances, and then swaps all the instances when everything is healthy

### All at Once Deployment

- Fastest deployment type
- Application has downtime
- Good for development environments
- No additional cost

![]()

### Rolling Deployment

- Application is running below capacity for some time
- Can set the bucket size
- Application is running both versions simultaneously
- If you set a small bucket size, it will be a very long deployment

![]()

### Rolling Deployment With Additional Batches

- Application is always running at capacity (sometimes above capacity)
- Can set the bucket size
- Application is running both versions simultaneously
- Small additional cost
- Additional batch is removed at end of the deployment
- Longer deployment
- Good for production environment

![]()

### Immutable Deployment

- Zero downtime
- High cost, double capacity
- New code is deployed to new intances on a temporary ASG
- Quick rollback in case of failures (just terminate the new ASG)
- Great for production environment

![]()

### Blue Green Deployment

- Not a direct feature of EB
- Zero downtime and release facility
- Create a new stage environment and deploy v2 there
- The new environment (green) can be validated independently and roll back if issues arise
- Route 53 can be setup with weighted policies to redirect a little bit of traffic to the stage environment (green) to test
- Using EB we can swap URLs when done with the environment testing so that new environment gets URL of old one -- thereby leaving users unaffected (Note: "swap" here refers to a `CNAME` swap)

![]()

## Elastic Beanstalk CLI

- We can install an additional CLI called "EB ClI" which makes working with Beanstalk from the CLI easier
- It is helpful for your automated deployment pipelines

## Elastic Beanstalk Deployment Process

- Describe dependencies
- Package code as zip
- Via console: you can upload zip file (creates a new app version) and then deploy
- CLI: create a new app version using CLI (uploads zip) and then deploy
- Note when app artifact is uploaded as zip, it actually goes to S3 and EB picks it up from there
- EB will deploy the zip on each EC2 instance, resolve dependencies, and start the application
- Note: this process can also be automated with CodePipeline and CodeBuild such that CodePipeline detects when there was a change to our source code (either on CodeCommit, GitHub, BitBucket) and packages the code & uploads the artifact to S3. From S3, CodeBuild can pick it up and run some predefined script to test / build our project and upload the new artifact again to S3. From there, CodePipeline can be configured so that Elastic Beanstalk picks up the artifact from S3 and then deploys it to our desired infrastructure as describe by our `.config` files in the `ebextensions` directory in our artifact

## Elastic Beanstalk Lifecycle Policy

- Elastic Beanstalk can store at most `1000` application versions
- If you don't remove old versions you won't be able to deploy anymore
- To phase out old application versions use a **lifecycle policy**
  - Based on time (old versions are removed)
  - Based on space (when you have too many versions)
- Versions that are currently used won't be deleted
- There is an option not to delete the source bundle in S3 to prevent data loss

## Elastic Beanstalk extensions

- All the parameters set in the UI can be configured with code using files
- Requirements:
  - Configuration files must all end with `.config` and they must all be in the `.ebextensions/` directory in the root of your source code
  - Able to modify some default settings using the `option_settings`
  - Ability to add any resource (e.g., RDS, ElastiCache, DynamoDB, etc..)
- **Note:** resources managed by `.ebextensions/` get deleted if the environment goes away

## Elastic Beanstalk Under the Hood

- Under the hood Elastic Beanstalk relies on CloudFormation
- CloudFormation is used to provision other AWS services (infrastrucure as code)
- Use case: you can define CloudFormation resources in your `.ebextensions/` to provision any AWS resource

## Elastic Beanstalk Cloning

- You can clone an environment with the same exact configuration
- All resources and configurations are preserved. For example:
  - Load balancer type and configuration
  - RDS database type (but the data **is not** preserved)
  - Environment variables, etc..

## Elastic Beanstalk Migration: Load Balancer

After creating an Elastic Beanstalk environment **you cannot change the Elastic Load Balancer type** (only the configuration).

To migrate:
  1. Create a new environment with the same configuration except the Load Balancer (to do this we can't use the "clone" feature described above since that would copy the same exact LB type and configuration -- so we have to create a new EB environment with the desired configuration)
  2. Deploy your application onto the new environment
  3. Perform a CNAME swap or Route 53 update

## RDS with Elastic Beanstalk

- RDS can be provisioned with Beanstalk which is great for a dev / test environment
- This is not great for prod as the database lifecycle is tied to the Beanstalk environment lifecycle
- The best way to do it for a production environment is to separately create an RDS database and provide our EB application with the connection string instead

### How to Decouple RDS if it is Already In Our Beanstalk Stack

1. Create a snapshot or RDS DB instance (as a safeguard -- so we have backed data in case something goes wrong)
2. Go to the RDS console and protect the existing RDS instance (that is part of our EB stack) from deletion
3. Create a new EB stack environment without RDS and point you application to the RDS instance from step (2)
4. Perform a CNAME swap so that the new EB load balancer takes the URL of our original EB load balancer -- this way consumers are not affected since the load balancer endpoint does not change (this is a blue / green deployment approach)
5. Terminate the old environment. Because we enabled RDS delete termination protection in step (2) the RDS instance will not be deleted
6. Because RDS has delete protection, the EB CloudFormation stack will be in a `DELETE_FAILED` state so we have to delete CloudFormation stack manually

![]()

## Elastic Beanstalk with Docker

### EB Single Container Mode

- Run your application as a **single Docker container**
- Either provide:
  - **Dockerfile:** Elastic Beanstalk **will build and run** the Docker container
  - **Dockerrun.aws.json:** describe where **already built** Docker image is
- **Note:** EB in single Docker container **does not use ECS**

### EB Multi Docker Container

- Multi Docker helps run multiple containers per EC2 instance in EB
- This will create for you:
  - ECS Cluster
  - EC2 instances configured to use the ECS Cluster
  - Load Balancer (in high availability mode)
  - Task definitions and execution
- Requires a config **Dockerrun.aws.json** at the root of the source code
- **Dockerrun.aws.json** is used to generate the **ECS task definition**
- Your Docker images must be pre-built and stored in AWS ECR or Docker Hub for example

![]()

## Elastic Beanstalk and HTTPS

- **Beanstalk with HTTPS:**
  - Load the SSL certificate onto the Load Balancer
  - Configure it from the code using `.ebextensions/securitylistener-alb.config`
  - SSL certificate can be provisioned using ACM (AWS Certificate Manager) or CLI
  - Must configure a security group rule to allow incoming traffic on port `443` (HTTPS port)
- **Beanstalk redirect HTTP to HTTPS:**
  - Configure your instances to redirect HTTP to HTTPS, or
  - Configure the ALB with a rule
  - Make sure health checks are not redirected (so they keep giving `200 OK`)

## Webserver vs Worker Environment

- If your application performs tasks that are long to complete, offload these tasks to a dedicated **worker environment**
- **Decoupling** your application into two tiers is common
- Example: processing a video, generating a zip file, etc..
- You can define periodic tasks in a file **cron.yaml**

![]()

## Elastic Beanstalk -- Custom Platform (Advanced)

- Custom Platforms are very advanced they allow to define from scratch:
  - OS
  - Additional software
  - Scrips that EB should run on these platforms
- **Use case:** app language is incompatible with Beanstalk and doesn't use Docker
- To create you own platform:
  - Define an AMI using **Platform.yaml** file
  - Build that platform using the **Packer software (open source tool to create AMIs)**
- Custom Platform vs Custom Image (AMI):
  - Custom Image is tweak an **existing** EB platform
  - Custom Platform is to create **an entirely new** EB platform
