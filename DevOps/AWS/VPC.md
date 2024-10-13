![[Pasted image 20241009142731.png]]

# AWS VPC (Virtual Private Cloud)

## Overview
An **AWS Virtual Private Cloud (VPC)** allows you to create a virtual network in the AWS cloud where you can define IP address ranges, subnets, route tables, and network gateways. VPCs enable you to have full control over your network environment, including resource placement, security, and internet access.

---

## Key Components

### 1. **VPC**
   - A **VPC** is a virtual network that closely resembles a traditional network.
   - Allows full control over networking settings such as IP range, subnets, and security.
   - VPCs are **region-specific**.

### 2. **Subnets**
   - Subnets divide the VPC into smaller networks.
   - Can be **Public** (connected to the internet) or **Private** (isolated from the internet).
   - Subnets are **AZ-specific** (Availability Zone).
   - AWS reserves 5 IP from each subnet ( first 4 & last 1)

### 3. **CIDR Block**
   - VPCs are assigned a **CIDR block** that defines the IP address range.
   - Example: `10.0.0.0/16`, allowing 65,536 IP addresses.
   - Subnets are created within the VPC's CIDR block.

### 4. **Route Tables**
   - Defines how traffic is routed within the VPC and beyond.
   - Each subnet is associated with a **route table**.
   - Routes to the internet or other VPCs are handled via the **internet gateway** or **VPC peering**.

### 5. **Internet Gateway (IGW)**
   - Enables **internet access** for resources in public subnets.
   - Public subnets need a route to the IGW for outbound and inbound traffic.

### 6. **NAT Gateway**
   - Allows **private subnets** to initiate outbound traffic to the internet (e.g., updates) without exposing the resources to inbound internet traffic.
   - Depends on specific AZ
   - bandwidth 5Gbps -> 100Gbps upon scaling
   - NAT Gateways are managed services; NAT instances can also be used but require manual management. ( below is older NAT instances)
    ![[Pasted image 20241013225304.png]]

### 7. **Security Groups**
   - Act as virtual **firewalls** to control traffic to/from instances.
   - State-based: rules allow/deny traffic both inbound and outbound.

### 8. **Network Access Control Lists (NACLs)**
   - Acts as a **stateless firewall** for controlling traffic to/from subnets.
   - NACL rules apply to both inbound and outbound traffic explicitly.
   - Applies for particular Subnet `NACL -> Subnet -> SG -> Instance`
   - On Ephemeral Ports `clients receiving ports, will be on range of 1024 - 65535`
		- Most cases ephemeral Ports will be required , so should allowed for clients port `i.e allowing ports range of 1024-65535 server:outbound & client:inbound`

### 9. **VPC Peering**
   - Allows private communication between two VPCs.
   - Can be within the same region or across different AWS regions.
   - Peered VPCs can route traffic to each other without using the public internet.
   > Note CIDR should not overlap

### 10. **Elastic IPs**
   - Static, public IPv4 addresses that can be attached to EC2 instances.
   - Helps maintain a consistent public IP address for resources.

### 11. **VPN Gateway & Customer Gateway**
   - Enable secure **VPN connections** between your on-premise network and the VPC.
   - Use cases include hybrid cloud environments.

---

## VPC Networking Basics

### **Private and Public Subnets**
- **Public Subnet**: Has a route to the Internet Gateway (IGW), allowing resources to communicate with the internet.
- **Private Subnet**: No direct route to the internet. Typically used for internal-facing resources like databases.

### **CIDR Ranges**
- VPC and subnets are defined by **CIDR blocks** (Classless Inter-Domain Routing).
- Subnet example: `10.0.1.0/24` (256 IP addresses).

---

## Key Features

### 1. **VPC Flow Logs**
   - Capture and monitor traffic flow in/out of VPC for analysis or security purposes.
   - Logs can be sent to CloudWatch or S3.

### 2. **VPC Endpoints**
   - Private connections to AWS services (e.g., S3, DynamoDB) from your VPC without traversing the internet.
   - Types:
     - **Interface Endpoint**: Uses an elastic network interface.
     - **Gateway Endpoint**: Used for S3 and DynamoDB.

### 3. **VPC Sharing**
   - Allows multiple AWS accounts to use subnets within a shared VPC.
   - Useful for multi-account environments.

---

## Use Cases

### 1. **Hosting Web Applications**
   - Public subnets host front-end web servers, while private subnets host backend services like databases.

### 2. **Hybrid Cloud**
   - Use a **VPN Gateway** or **Direct Connect** to extend on-premise networks to AWS VPC.

### 3. **Isolated Workloads**
   - Run highly secure or sensitive workloads within private subnets without internet exposure.

---

## Best Practices

- **Use multiple subnets** for high availability across Availability Zones (AZs).
- **Restrict access** using **Security Groups** and **NACLs** to ensure secure communication.
- **Split CIDR blocks** effectively to avoid running out of IP addresses in subnets.
- Enable **VPC Flow Logs** for traffic monitoring and troubleshooting.

---

## Common VPC Scenarios

### 1. **VPC with Public and Private Subnets (NAT Gateway)**
   - Public Subnet: Web servers, jump boxes.
   - Private Subnet: Databases, application servers.

### 2. **VPC Peering Between Two VPCs**
   - VPC A and VPC B communicate directly using private IP addresses.

### 3. **VPC-to-VPC VPN**
   - Two VPCs are connected via a VPN gateway to allow encrypted traffic between them.

---
### Bastion Hosts

- For accessing instances in private subnet
- they have their own security groups ( bastion security groups )
---

## Pricing

- VPC itself is free, but certain features have associated costs:
   - **NAT Gateway** (priced by the hour and GB transferred).
   - **VPN Gateway**.
   - **Data transfer charges** for traffic across availability zones or VPCs.

---

## Summary

AWS VPC provides a highly configurable and secure environment for deploying cloud-based applications. It allows you to isolate and control networking aspects like IP ranges, subnets, and security rules. By using tools like internet gateways, NAT gateways, security groups, and NACLs, you can build robust cloud networks tailored to specific needs.
