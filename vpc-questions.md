# AWS Certified Solutions Architect

## VPC Questions

#

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

#

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

#

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

#

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

#

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

#

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

#

At which EC2 instance states can the source/destination check attribute be changed? (Choose two):

1. When the NAT instance state is terminated (exluída)
2. When the NAT instance state is pending (iniciando)
3. When the NAT instance state is running (em execução)
4. When the NAT instance state is stopped (parada)

- a) 1 and 3 >> não
- **b) 3 and 4**
- c) 2 and 3 >> não
- d) 1 and 4 >> não

#


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

#

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

#

After creating a **VPC with CIDR 10.0.0.0/16**. with the lack of proper architecture, The AWS SysOps admin created one large **subnet of CIDR 10.0.0.0/16**. later on, another subnet was needed to host another tier of an application being deployed. The admin is **trying to create another subnet of CIDR 10.0.1.0/24**.

Can she create the second subnet without disrupting services to the first subnet?

1. Yes, she can configure the new subnet, and AWS will automatically adjust the VPC subnets so both can exist.
2. Yes, Edit the fist subnet from the console to make room for the second one
3. **No, It is not possible to create a second subnet as the intended one overlaps with the existing one.**
4. Yes, Delete the VPC and create a new one

_Choice Analysis_:

1. **Incorrect** - AWS VPC does not adjust subnets automatically
2. **Incorrect** - You can not adjust subnet size after creating it, you can delete it and create another
3. **Correct** - The first one took all the VPC CIDR block, hence, any other one will overlapping with it
4. **Incorrect** - Although this would work, but this would cause a major impact to the services/apps

**Quick Refresh**:
- You cannot create overlapping IP address subnet in a VPC
- All VPC subnets must be no-overlapping
- If you create one subnet equal in size to the VPC CIDR block, you will not be able to crete any other subnet in the VPC because any other one that belongs to the CIDR block will overlap with the first one
    - Solution - delte the one you created, and then carefully design and deploy your IP address scheme

#

Your AWS SysOps administrator created a **VPC with a public subnet**. He created and attached an **Internet Gateway to the VPC**, and launched an **EC2 instance with a public IP** in the subnet. He also **created a security group** for the EC2 instance. When **trying to connect to the EC2 instance from the Internet**, he was not able to. From the statements below, which could be a possible reason for his inability to connect?  **(Choose 2)**

1. There is no entry in the route table pointing to the internet gateway as a Target
2. The admin did not configure the security group after he created it
3. The security group is denying any outbound traffic to the Internet
4. The admin forgot to create a NACL for the EC2 instance

Options:

1. 3 and 4
2. **1 and 2**
3. 1 and 3
4. 2 and 3

_Choice Analysis_:
- **Correct** - For the subnet to the public, an destination: 0.0.0.0/0 pointing at the IGW must be present in the route table of the subnet hosting the EC2 instance
- **Correct** - Custom (non-default) security group would not have SSH allowed inbound
- **Incorrect** - Security groups alwasy allow outbound traffic by default
- **Incorrect** - Default NACL would apply in this case, which allows all inbound/outbound

#

You created **a subnet in a custom VPC** and launched **an EC2 instance** in that subnet. During the EC2 instance creation, using AWS console, **you did not choose the option to assign a public IP address to your instance**. This instance now **needs access to the Internet**, but it **has no public IP address**.

How would you solve this internet connectivity issue for this EC2 instance?

1. The instance will always have a public DNS attached to the instance by default
2. Allocate and attach an Elastic IP directly to the instance
3. The instance would not launch if the public IP is not assigned
4. **Create an internet gateway, attach it to the VPC, do the needed route table configuration for a public subnet. Adjust security group, and NACLs configurations to facilitate this, and finally, attach an elastic IP to the instance to connect to the Internet**

_Choice Analysis_:
1. **Incorrect** - This depends on the instance, how many interfaces, public IP assigned or not.. etc
2. **Incorrect** - Public and Elastic IP addresses are configured on the IGW and NAT'ed to the Instance's private IPv4 address
3. **Incorrect** - Instances can launch in Private subnets without a public IP
4. **Correct** - These are the steps needed to allow traffic to the Internet

**Quick Refresher**:
- In order for an instance to be access or access the Internet:
    - It has to be in a public subnet with a Public or Elastic IP
        - A public Subnet is a subnet created which has:
            - A route table with an Entry to an Internet Gateway
            - The Internet Gateway must be attached to the VPC
            - Security group and NACL must allow the traffic as necessary

#

Your client plans to **connect their Data Center to their AWS VPC** in preparation for an application launch in **few months**. The application they are launching is chatty and has components in AWS and in the data center, and **will be hosted in private AWS subnets in their AWS VPC**. It also **requires bandwidth and latency guarantees at all times**. The solution has to be **fault tolerant**. Which connectivity method would you recommend for them?

1. One VPN connection with two tunnels between one Customer Gateway and one VGW router on AWS side
2. Two Public VIFs over two Direct connect connections. From two Customer routers to two different DX routers
3. **Two Direct connect connections using two Customer routers and two private VIFs to two different Direct connect routers**
4. One direct connect connection with one private VIF, and a backup VPN connection from two customer routers

**Important question points**:
- Application requires bandwidth and latency guarantees
- Application will be hosted in private subnets in the VPC
- Solution must be fault tolerant
- Application will launch in few months (They have time)
- Single choice

_Choices Analysis_:
- 1. **Incorrect** - VPN will not guarantee bandwidth and latency, it is Internet based
- 2. **Incorrect** - The application will be hosted in private subnets, you need private VIF
- 3. **Correct** - 
- 4. **Incorrect** - VPN backup will not provide bandwidth and latency guarantees

#

Your client has a **multi AZ infrastructure on AWS**, and plans , in few months, to have a centralized, custom, dashboard **in the client’s data center**. The dashboard will need to interact with the multi AZ infrastructure. Data from the Multi AZ will be pulled from the data center. **Latency and performance (bandwidth) are key**. The solution needs to be up and running **within few months**.

How would you architect the solution?

1. Use redundant VPN connections to two VGW routers in the region, this should give you access to the infrastructure in all AZs
2. **Use direct connect connection to the client VPC, as this will provide access to all AZs in the region, and will also provide better bandwidth and lower latency**
3. Use one direct connect connection from the data center to each AZ in the region
4. You cannot interact with multiple AZs from one location

**Important question points**:
- Multi AZ infrastructure on AWS that will be connected to the client daa center
- Time frame is few months
- Solution must allow the data center to pull data from multiple AZs
- Requires bandwidth and latency guarantees

_Choices Analysis_:
1. **Incorrect** - VPN will not guarantee bandwidth and latency, it is Internet based
2. **Correct** - 
3. **Incorrect** - You do not need a DX connection per AZ
4. **Incorrect** - You can

#

Using **AWS direct connection, with public and private VIFs** you can: (Choose 3)

1. Connect to AWS services over the private VIF
2. Connect to your private VPC subnets over the public VIF
3. **Connect to your private VPC subnets over the private VIF, and to Public AWS services over the public VIF**
4. Substitute your internet connection at your DC with AWS’s public Internet through the use of a NAT gateway in your VPC
5. **Once connected to your VPC through Direct conect you can connect to all AZs within the region**
6. **Using IPSec VPN you can connect over the public VIF to remote AWS regions as well**

Options: 
1. 1, 2 and 3
2. 3, 4 and 5
3. **3, 5 and 6**
4. 2, 3 and 4

_Choice Analysis_:
1. **Incorrect** - This can be done over the public VIF
2. **Incorrect** - This can be done over the private VIF
3. **Correct** - 
4. **Incorrect** - You can't access the AWS internet through the Direct Connect Connection
5. **Correct** - 
6. **Correct** - 

**Quick refresher**:
- DX is a direct connection (not internet based) and **provides for higher speeds (bandwidth), less latency and higher performance** than Internet
- A Virtual Interface (VIF) is basically a 802.1Q VLAN mapped from the customer router to the Direct Connect router
- You need one private VIF to connect to your private VPC subnets, and one public VIF to connect your AWS public services
- You **CANNOT** establish layer 2 over your DX connection
- You **CANNOT** use a NAT instance/gateway in your VPC over the direct connect connection

#

Your company has **peered two VPCs in the same region, VPC-A and VPC-B**. Moreover, **your company’s HQ is connected to VPC-A using a VPN connection**. You want to make this setup more **fault tolerant**, and ensure that company HQ has connectivity to VPC-A at all times.

How can you architect this solution **quickly and cost effectively**?

1. Peer the corporate network to VPC-B
2. Connect Corporate network to VPC-B by a VPN connection such that it has another path to VPC-A
3. **Configure a second VPN connection between HQ and VPC-A from another customer gateway at the HQ**
4. Configure a second VPC peering between VPC-A and VPC-B

**Important question points**:
- Solution is not fault tolerant as it stands
- HQ connectivity to VPC-A is critical
- Solution needs to be quick to deploy and cost effective

_Choices Analysis_:
1. **Incorrect** - You **CANNOT** do VPC peering between a VPC and an external network
2. **Incorrect** - Edge to Edge routing is not supported - HQ can't access VPC-A through VPC-B
3. **Correct** - 
4. **Incorrect** - You can not configure a second peering connection between two VPCs

**Quick Refresher**:
- VPC peering does not support edge-to-edge routing.
- VPC-A and VPC-B are peered, VPC-A is also connected to the Corporate Network by a VPN or Direct Connect Connection
- Corporate network **CANNOT** access VPC-B by being connected to VPC-A, because VPC peering will not allow VPC-B routes to be advertised to Corporate network and vice versa
- Edge to edge routing here refers to routing external traffic (to the peered VPCs) through the VPC peering connection

#

Your company has **seven offices including HQ**. The company just implemented an application on AWS in a VPC. The application will be **accessed by company employees from all seven locations**. Latency and performance **are not a big concern**, however, the solution needs to be **fast, easy to deploy, and cost effective**.

How would you architect the solution?

1. Deploy an OpenSSL server on an EC2 instance in your VPC. Have the employees establish SSL based remote access when they need to access the application
2. **Establish a site-to-site IPSec VPN from each location to the VPC’s VGW and adjust routing to allow access to the application**
3. Establish a Direct Connect connection from each location to the VPC
4. You can not connect multiple locations concurrently to your AWS VPC

_Choice Analysis_:

1. **Incorrect** - despite possible but this involves an EC2 instance, administration hassle
2. **Correct** - VPN CloudHub is the solution to this case
3. **Incorrect** - Costly and not quick
4. **Incorrect** - You can

**Quick Refresher**:
- You can **have up to 10 IPSec** connections per VGW (soft limit can be increased by contacting AWS)
- VPN based Hub and Spoke connectivity to a common VGW
- Can mix DX connections with VPN connections
- Spokes can communicate with each other and with the VPC

#

Your company just implemented an HR application on **AWS in a VPC**. The application will provide payroll and benefits information to the employees and **needs to be accessed from all twenty Company locations**. Latency and performance **are not a big concern**, however, the solution needs to be **fast and easy to deploy and cost effective**. Your solutions should also allow the **twenty locations to communicate with one another**.

How would you architect the solution?

1. Deploy an OpenSSL server on an EC2 instance in your VPC. Have the employees establish SSL based remote access when they need to access the HR application
2. Establish a site-to-site IPSec VPN from each location to the VPC’s VGW and adjust routing to allow access to the application
3. You can not connect multiple locations concurrently to your AWS VPC
4. **You need to contact AWS first to increase the 10 VPNs per VGW limit, then configure VPN Cloudhub to connect the 20 locations**

_Choice Analysis_:

1. **Incorrect** - Requires EC2 instance and administration hassle. Also it is not site-to-site
2. **Incorrect** - By default AWS allows only 10 VPN connections per VGW
3. **Incorrect** - Costly and not quick
4. **Correct**  