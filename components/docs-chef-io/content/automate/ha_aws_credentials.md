+++
title = "HA AWS Credentials"

draft = true

gh_repo = "automate"

[menu]
  [menu.automate]
    title = "HA AWS Credentials"
    parent = "automate/install"
    identifier = "automate/install/ha_aws_credentials.md HA AWS Credentials"
    weight = 250
+++

## Amazon Web Services Credentials

no 2 VPCs can communicate directly 

<!-- in config.toml file
region = "ap-south-1"
aws_vpc_id  = "vpc-8d1390e5"
aws_cidr_block_addr  = "172.31.128.0"
# ssh key pair name in AWS to access instances
ssh_key_pair_name = "cp-a2ha" (this is same key-pair name what we have used to provision the bastion ec2 machine).we have define the ssh_key_file, this should point to the same key file. -->


We need to create the certificate for the DNS.
Harden the OS, which basically refers to increasing the security which has been provided by the OS.
Specify appropriate security groups or create a security group for the bastion host.
This will open up the port 22 which is usually used with SSH.
Select a source, which is done to ensure that relevant people (who have access to add their IPs) have access to the Bastion host.
The security groups of the current instances have to be changed to make sure that inbound SSH (if any) can be accessed through the Bastion Host's IP address only.
The local ~/.ssh/config file has to be edited to reflect the bastion host name, username, and a 'Yes' value for the ForwardAgent field. This is used to set up the SSH forwarding via the local machine to the bastion host so that the file used to access the EC2 instance is made available only when the user tries to connect to one of the servers.
Username refers to the person who has the rights to login to the server. Hostname refers to the IP address of the bastion host.
This makes sure that the user can SSH into the Bastion server by just typing 'ssh bastion' from the command line interface.
Bastion Host needs to be accessed with the help of SSH. into an existing server instance, and this way, a much tighter security would be built into the servers, by making these servers accessible only through a Bastion host.
Copy any available VPC ID from aws web portal
Navigate to the Subnet option from the Virtual Private Cloud menu of the AWS console.
Note down the unique Ipv4 CIDR field in the subnet section. We need to derive a value for 'aws_cidr_block_addr'.

Follow the below approach to do so

https://www.calculator.net/ip-subnet-calculator.html?cclass=a&csubnet=20&cip=172.31.0.0&ctype=ipv4&printit=0&x=82&y=36
https://ccna-200-301.online/network-documentation/


Subnet field: iI your vpc IPv4 CIDR block is 172.31.0.0/16 then just add plus 2 to make 18 because we have set /20 in our system.

IP Address: Write down 172.31.0.0 from 172.31.0.0/16 from IPv4 CIDR block and press calculate.

From the results, note the available Network Address field as a value for 'aws_cidr_block_addr= ' field.

Specify CIDR


#### VPC Setup

- VPC Services - of all regions - region in which you are supposed to create the infrastructure. Select the available VPC or create a new one.Which will have CIDR block and Ips available. AWS account has limit of 20 VPCs per region.  no 2 VPCs can communicate directly

#### Certificates

Certificate is optional. Default certificate of Postgres n es. Required for load balancer. Arn link of the aws certificate customer to provide .. put the certificate in the certificate manager. Customer can give theirs. They can provide to the installation team

- Key Pairs

As a bastion server, it should have the public access with a public IPv4 address.

Arn link in config.toml… default certificate we can use - certificate manager — 

- full access is required for s3 backup


## Amazon's Virtual Private Cloud (VPC)

Amazon VPC, a virtual network dedicated to your AWS account that enables you to launch AWS resources into a virtual network. This virtual network resembles a traditional network that you had operate in your own data center, with the benefits of using the scalable infrastructure of AWS.

Amazon VPC is the networking layer for Amazon EC2. Amazon Elastic Compute Cloud (Amazon EC2) provides scalable computing capacity in the Amazon Web Services (AWS) Cloud. Using Amazon EC2 eliminates your need to invest in hardware up front, so you can develop and deploy applications faster. You can use Amazon EC2 to launch as many or as few virtual servers as you need, configure security and networking, and manage storage. Amazon EC2 enables you to scale up or down to handle changes in requirements or spikes in popularity, reducing your need to forecast traffic.

VPC creates an isolated virtual network environment in the AWS cloud, dedicated to your AWS account. Other AWS resources and services operate inside of VPC networks to provide cloud services. AWS VPC looks familiar to anyone used to running a physical Data Center (DC). A VPC behaves like a traditional TCP/IP network that can be expanded and scaled as needed. However, the DC components you are used to dealing with—such as routers, switches, VLANS, etc.—do not explicitly exist in a VPC. They have been abstracted and re-engineered into cloud software.

All VPCs are created and exist in one—and only one—AWS region. AWS regions are geographic locations around the world where Amazon clusters its cloud data centers.

The advantage of regionalization is that a regional VPC provides network services originating from that geographical area. If you need to provide closer access for customers in another region, you can set up another VPC in that region.

This aligns nicely with the theory of AWS cloud computing where IT applications and resources are delivered through the internet on-demand and with pay-as-you-go pricing. Limiting VPC configurations to specific regions allows you to selectively provide network services where they are needed, as they are needed.

Each Amazon account can host multiple VPCs. Because VPCs are isolated from each other, you can duplicate private subnets among VPCs the same way you could use the same subnet in two different physical data centers. You can also add public IP addresses that can be used to reach VPC-launched instances from the internet.

You can modify or use that VPC for your cloud configurations or you can build a new VPC and supporting services from scratch.

### Amazon's Virtual Private Cloud (VPC) Limit

The default limit to create a VPC in a region is 5. However, if the VPCs used in the respective region is exhausted, you can increase the limit in your AWS account. Chef Automate HA on AWS deployment creates two VPCs, each for the bastion host and for the rest of the node in a cluster.

{{< note >}}

You require a minimum of three node clusters for ElaticSearcg and Postgres-sql instances.

{{< /note >}}

AWS limits the size of each VPC; a user cannot change the size once the VPC has been created. Amazon VPC also sets a limit of 200 subnets per VPC, each of which can support a minimum of 14 IP addresses. AWS places further limitations per account / per region, including limiting the number of VPCs to five, the number of Elastic IP addresses to five, the number of Internet gateways per VPC to one, the number of virtual private gateways to five and the number of customer gateways to 50.

CIDR block -Classless Inter-Domain Routing. An internet protocol address allocation and route aggregation methodology. For more information, see Classless Inter-Domain Routing in Wikipedia.
Subnet - A range of IP addresses in your VPC.

VPC IP address ranges are defined using Classless interdomain routing (CIDR) IPv4 and IPv6 blocks. You can add primary and secondary CIDR blocks to your VPC, if the secondary CIDR block comes from the same address range as the primary block.