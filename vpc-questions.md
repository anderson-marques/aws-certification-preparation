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

#### Question 7

You are the AWS architect at YCDIT2, Inc. you have been tasked to design and launch an EC2 NAT instance in a public subnet in your client's VPC. After creating and successfully testing the NAT instance. You have also updated your private subnet's rout table such that the NAT device is the target for traffic destined to the Internet. However, the private subnet EC2 instances are still not able to connect to the Internet for updates and patch download.

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

