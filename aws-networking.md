
## Networking Basics

*Analogy: AWS is the whole office building, VPC is a floor, Subnets are conference room/suites.*

### VPC
  - VPCs(Virtual Private Cloud) are virtually isolated network spaces in AWS. Each AWS account has a default VPC and a soft limit of 5 VPCs (can be increased by request)
  - A VPC is defined by a CIDR block. Ex: `10.0.0.0/16` (`/16` is the subnet mask) has `2^(32-16) = 65,536`(min: `/28`, max:`/16`) allocatable IPs. Subnets should be part of this CIDR block
  - **Configurable params:** CIDR Block
### Subnets:
  - Subnets offer another level of isolation inside a VPC. Why do we need them? To secure our resources. Entities such as Databases, servers do not need to be accessed by the outside world - only through API Gateways and Load balancers. 
  - There are two types of subnets:
    - **Public Sunbet**: resources placed in this can be accessed from anywhere. Each resources placed inside the public subnet is connected to an Internet Gateway(an entity that gives **two-way access** to the internet). Typical resouces that are placed here are Bastions, Load Balancers, API gateways, NAT Gateways.
         - Configuration: 
         
        | Destination | Target |
        | ---- | ---- |
        | within the VPC (ex: 10.1.2.3) | local - routes within the VPC |
        | outside - 0.0.0.0/0 | IGW - internet | 
    - **Private Sunbet**: resources placed in this can be accessed only within the VPC. Each resources placed inside the private subnet is connected to an NAT Gateway(an entity that gives **one-way access** to the internet). Typical resouces that are placed here are servers, Databases, compute etc.
         - Configuration: 
         
        | Destination | Target |
        | ---- | ---- |
        | within the VPC (ex: 10.1.2.3) | local - routes within the VPC |
        | outside - 0.0.0.0/0 | NAT - Located in the public subnet | 
 - **Configurable params:**
    - CIDR block (must be within the VPC CIDR block)
    - Availability Zone (VPCs span across AZs but subnet can only be in one AZ)
    - Network ACL: the first firewall of the subnet. Here you can specify which ports to open to and from different sources. By default all traffic is allowed inbound and outbound.

## Gotchas & things that took days to understand (: 

1. Flow of an incoming request to a resource in a Public Subnet\
   `request from internet -> Public Subnet ACL (inbound rules) -> Resource Security Group (inbound rules) -> OS firewall`
2. Flow of an incoming request to a resource in a Private Subnet\
   `request from internet -> Public Subnet ACL (inbound rules) -> -> NAT Gateway -> Private Subnet ACL (inbound rules) -> Resource Security Group (inbound rules) -> OS firewall`
3. Flow of outgoing request from a resource in a Public Subnet to the internet\  
   `OS firewall -> resource Security Group (outbound rules) -> Public Subnet ACL(outbound rules) -> Internet Gateway`
4. Flow of outgoing request from a resource in a Private Subnet to the internet\
   `OS firewall -> resource Security Group (outbound rules) -> NAT Gateway -> Public Subnet ACL(outbound rules) -> Internet Gateway`
5. Lambdas **must** be placed in private subnets with NAT gateway connections if they need to access the internet. This is because Lambdas can *only* have private IPs. The default route for a resource with a private IP in a public subnet is an IGW so packets will be dropped at the gateway. The default route for a resource with a private IP in a private subnet is a NAT gateway which is then connected to the IGW.
