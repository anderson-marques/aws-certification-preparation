# AWS Certified Solutions Architect

## Preparação para a certificação


#### Question 1

What can you do to ensure that only a specific list of Internet IP addresses can access your web tier, of two tier application you are hosting at AWS VPC. Your web servers will be served by an Internet-facing Elastic Load Balancer (ELB).

Which two ways of the below would allow you to accomplish this (Choose 2 answers)

1. Configure the VPC internet gateway to allow incoming traffic from these IP addresses
2. **Configure your ELB to send the X-forwarded for headers and web servers to filter traffic based on the ELB's "X-forwarded-for" header.**
3. **Configure the ELB security group to allow traffic from this list of IPs and deny all outbound traffic**
4. Configure a VPC NACL to allow web traffic from the list of IPs and deny all outbound traffic.

Choices Analysis:

1. **Incorrect** - IGW is an AWS services and customer do not have access to it
2. **Correct** - If the web app can do this level of control
3. **Correct** - ELB can have a security group assigned which can be configured with Allow rules for Customer's IP addresses
4. **Incorrect** - NACL is stateless if you deny all outbound traffic the return traffic will be dropped

_Quick refresher:_

- ELB can be assigned a Security Group
- ELB can be configured to forward the client/initiator's information including source IP address using `X-Forwarded-For` headers

#### Question 2

You are the AWS architect at YCDIT2, Inc. Your client has a **VPC with public and private subnets** created by the **VPC wizard**. The **VPC CIDR is 10.0.0.0/16**. The **public subnet is 10.0.1.0/24**. The architecture you put together includes deploying a web server in the public subnet, receiving **HTTP traffic on port 80**; it also includes a Database server tier in the **private subnet receiving traffic on port 3306**. The client SysOps is configuring a **security group for the public subnet** called **WebSG**, and the private subnet's security group called **DbSG**.

Which of the below entries is required in the web server security group?
1. **Destination: DB Security group ID (DBSG), Port: 3306, Direction: Outbound**
2. Destination: 0.0.0.0/0, Port: 80, Direction: Outbound
3. Source 10.0.1.0/24, Port: 3306, Direction: Inbound
4. Source 10.0.0.0/16, Port: 80, Direction Inbound

Choices Analysis:

1. **Correct** - This allows the DbSG to be the destionation on port 3306, it allows traffic destined to instances in the DbSG with a destination port 3306 (DB port)
2. **Incorrect** - The web layer will be receiving HTTP traffic from the internet, not sending it out to the Internet
3. **Incorrect** - CIDR 10.0.1.0/24 was not mentioned in the question body.
4. **Incorrect** - HTTP traffic will not be received on WebSG from the public subnet

_Quick refresher:_
- You can use Security Group names as the source of destination in other security group rules
- You can use the security group name as the source in its own inbound security group rules
- Security groups are directional and can use allow rules only
- A security group set of rules ends with an implicit deny any

#### Question 3

Which of the below statements is true for **any VPC security group**, by default, **when it is created**?

1. All inbound traffic rule will be explicitly denied
2. All inbound traffic is allowed by default
3. **All outbound traffic is allowed by default**
4. Traffic to the internet gateway is allowed by default

Choices Analysis:

1. **Incorrect** - Security groups do not have explicity deny
2. **Incorrect** - All inbound traffic is denied by default in a Custom security group
3. **Correct** - By default there is a 0.0.0.0/0 on All protocols/ports allowed by default - outbound
4. **Incorrect** - Security Group do not have such a default rule

_Quick refresher:_

- Custom security groups in a VPC will always
    - No inbound rules - basically all inbound traffic is denied by default
    - All outbound traffic is allowed by default
- Default security groups in a default or custom VPC, will have: 
    - Inbound rules allowing Instances assigned the same security group to talk to one another
    - All outbound traffic is allowed  
- Security groups have allow only rules, no **explicit deny rules**
- A security group set of rules ends with an **implicit deny all**

#### Question 4

You created a **VPC with both public and private subnets**. The **VPC CIDR is 10.0.0.0/16**. The **private subnet is 10.0.1.0/24** and the **public subnet is 10.0.0.0/24**. The goal is to host a web server int the public subnet **receiving traffic on port 80**, and a DB server in the **private subnet receiving traffic on port 3306**. The database server will require in-frequent Internet access for patching and updates. When you are configuring the **security group of the NAT instance (NATSG)**, which of the below mentioned entries is not required?

1. Allow Source: 10.0.1.0/24, Direction: Inbound, Port: 80
1. Allow Destination: 0.0.0.0/0, Direction: Outbound, Port: 80
1. **Allow Source: 10.0.0.0/24, Direction: Inbound, Port: 80**
1. Allow Destination: 0.0.0.0/0, Direction: Outbound, Port: 443

Choices Analysis:

1. **Correct** - In order to allow private subnet request to NAT
2. **Correct** -In order to NAT request to the Internet
3. **Incorrect** - Public Subnet does not need use NAT
4. **Correct** - In order to NAT request to the Internet using SSL/TLS

_Quick refresher:_
- Security groups have inbound and outbound rules
- Security groups can only have Allow rules
- Draw in your mind, to understand the scenario better
- Focus on Inbound and Outbound traffic
- Private subnet instances will want to access websites on the internet (HTTP or HTTPs)
- Internet destinations when are not listed has to have 0.0.0.0/0 for all internet destinations
- NAT instances serves the private subnet's instances and not the public subnet's instances

#### Question 5

Your client deployed a **three-tier web application** in a VPC with a **CIDR block of 10.0.0.0/27**. The client launched **four web servers behind an ELB**, **four application servers**, **two database servers**, and **two NAT instances** are already deployed in the VPC across two AZ's, for a total of **twelve EC2 instances**. Route53 is used as the DNS. When trying **to double the existing fleet of web/app instances** to keep up with the gradual increase in web traffic, not all the required instances were created/launched successfully.

Which of the following could be the root cause? (Choose 2)

1. The IP address reserved by AWS services in each subnet for DNS has taken some of the available IP addresses in each subnet, hence, why instances failed to launch.
2. **ELB scaled up to keep up with the web tier load, which created more ELB nodes, taking more of the available IP addresses in the subnet where ELB is defined**.
3. **AWS reserves the first four and the last IP address in each subnet's CIDR block so you do not have enough addresses left to launch all of the new EC2 instances**.
4. New instances failed to launch because the incoming web traffic caused the existing instance CPU utilization to increase and the request could not get through.

Choices Analysis:

1. **Incorrect** - AWS reserves more than one IP 
2. **Correct** - ELB use until 5 instances to scale up or down
3. **Correct** - AWS reserves these IPs in each subnet
4. **Incorrect** - Non sense. CPU utilization do not have importance in this scenario

_Quick refresher:_
- When an ELB is deployed in a VPC, the ELB service itself launches ELB nodes. Depending on your traffic increase, the ELB service will continue to launch new ELB nodes eating up from your subnet available IP addresses, in the subnet where you defined the ELB in each AZ
    - The more the traffic, the more IP addresses are taken from your subnet available IP's
    - AWS suggests that you account for up to 8 IP addressess in an AZ for scaling
- The subnet is bound to an AZ, and can not stretch between AZ's
- In a /28 subnet in a VPC, you actually have only 11 IP addresses available for instances and ELB

#### Question 7

You have been tasked to design and launch an EC2 NAT instance in a public subnet in your client's VPC. After creating and successfully testing the NAT instance. You have also updated your private subnet's rout table such that the NAT device is the target for traffic destined to the Internet. However, the private subnet EC2 instances are still not able to connect to the Internet for updates and patch download.

Which of the following steps could be a possible reason for this problem?

1. NAT instance is launched with only one ENI in the public subnet
2. The NAT instance has not been configured with the proper NAT rules to process the private instance's traffic inteded for the internet
3. The NAT instance will not work, you need to configure static, one-to-one NAT on the VPC Internet Gateway for private subnet's instances to connect to the internet
4. **Disabling the Source/Destination Check attribute on the NAT instance**


Choices Analysis:

1. **Incorrect** - NAT instances can do its functions with one ENI (Primary Interface or eth0)
2. **Incorrect** - NAT AMIs has the required rules/configurations to performe the assigned PAT (Port Address Translation) job
3. **Incorrect** - NAT Instance does PAT function and this is enough to do the job required
4. **Correct** - This is one possible reason (besides checking the Security Groups and NACLs)

#### Question 8

At which EC2 instance states can the source/destination check attribute be changed? (Choose two):

1. When the NAT instance state is terminated (exluída)
2. When the NAT instance state is pending (iniciando)
3. When the NAT instance state is running (em execução)
4. When the NAT instance state is stopped (parada)

- a) 1 and 3 >> não
- **b) 3 and 4**
- c) 2 and 3 >> não
- d) 1 and 4 >> não

#### Question 9:

Using the VPC wizard, you have created a VPC with CIDR 10.0.0.0/16 with a VPN-only private subnet and Hardware VPN Access connection. You need to connect to an instance in the private subnet over SSH.

How should you define the instance’s security group rule to allow SSH?

1. **Allow Inbound traffic for SSH (port 22) from the corporate network**
2. Allow port 22 on the security group of the VPN-only subnet to allow SSH inbound
3. Create a public subnet, Implement a NAT instance and use it to connect to the VPN-only subnet instances
4. Allow Inbound traffic on port 22 to allow you to connect to a private subnet over the Internet

Choices Analysis:
1. **Correct** - From the corporate network, connect to the private subnet over port 22
2. **Incorrect** - There is no such a thing as security group of a VPN subnet
3. **Incorrect** - NAT instances are not for Internet Initiated connections
4. **Incorrect** - The VPC has not been configured with a public subnet for Internet Access

#### Question 10:
You have created a VPC with CIDR 10.0.0.0/24. The VPC has two subnets: public (10.0.0.0/25) and private (10.0.0.128/25). For an anticipated project you want to increase the CIDR range your VPC CIDR block, How can you do this?

1. Change the subnet sizes to /28 subnets, then you will have more room to grow your VPC CIDR
2. You can always change a VPC's original CIDR block as needed
3. You can add additional VPC CIDR blocks, but can’t change the existing one
4. Delete all the subnets first, only then you can modify the size of the VPC

Choices Analysis:
1. **Incorrect** - VPC CIDR block can not be changed. 
2. **Incorrect** - VPC CIDR block can not be changed. 
3. **Correct** - You can add additional VPC CIDR blocks, but can't change the existing one
4. **Incorrect** - You still can't change the VPC CIDR

