# AWS Networking: Comprehensive Architecture and Concepts Guide

## Table of Contents
1. [Virtual Private Cloud (VPC)](#virtual-private-cloud-vpc)
2. [Subnets](#subnets)
3. [Security Groups](#security-groups)
4. [Network ACLs](#network-acls)
5. [IP Addressing Fundamentals](#ip-addressing-fundamentals)
6. [CIDR Block Notation](#cidr-block-notation)
7. [Ports and Protocols](#ports-and-protocols)
8. [Best Practices](#best-practices)
9. [Step by Step Guide](#step-by-step-configuration-guide)
10. [Network ACLs vs Security Groups](#security-groups-vs-network-acls-in-aws)
11. [Configure EC2](#when-creating-ec2-instance)

## Virtual Private Cloud (VPC) 🌐

### Definition
A Virtual Private Cloud (VPC) is an isolated network environment in the AWS cloud, providing complete control over your virtual networking infrastructure.

### Key Characteristics
- Logically isolated section of AWS cloud
- Supports IPv4 and IPv6 addressing
- Can span multiple Availability Zones
- Configurable IP address range (CIDR block)

### Technical Components
- **Internet Gateway**: Enables communication between VPC and internet
- **Virtual Private Gateway**: Supports VPN connections
- **Peering Connections**: Connect VPCs across different accounts/regions
- **NAT Gateway**: Allows private subnet resources to access internet

### Configuration Example
```
VPC CIDR: 10.0.0.0/16
Total IP Range: 65,536 possible addresses
Subnets:
- Public Web Subnet: 10.0.1.0/24
- Private Database Subnet: 10.0.2.0/24
```

## Subnets 🏘️

### Definition
Subnets are subdivisions within a VPC that segment and organize network resources.

### Types of Subnets
1. **Public Subnet**
   - Directly accessible from the internet
   - Has route to Internet Gateway
   - Hosts web servers, load balancers

2. **Private Subnet**
   - No direct internet access
   - Hosts databases, backend services
   - Accessed through NAT Gateway/VPN

### Subnet Creation Guidelines
- Allocate 10-20% extra IP space
- Create subnets across multiple Availability Zones
- Separate public and private resources
- Use smallest practical CIDR block

### Configuration Example
```
VPC: 10.0.0.0/16
Public Subnets:
- 10.0.1.0/24 (Web Layer)
- 10.0.2.0/24 (Public DMZ)

Private Subnets:
- 10.0.10.0/24 (Application Layer)
- 10.0.20.0/24 (Database Layer)
```

## Security Groups 🚪

### Definition
Security Groups act as instance-level firewalls, controlling traffic for individual AWS resources.

### Key Features
- Stateful firewall
- Default: Deny all inbound, allow all outbound
- Supports protocol-specific rules
- Can reference other security groups

### Rule Configuration Example
```
Web Server Security Group:
- Inbound Rules:
  * Allow HTTP (Port 80)
  * Allow HTTPS (Port 443)
  * Allow SSH (Port 22) from specific IP range

Database Server Security Group:
- Inbound Rules:
  * Allow MySQL (Port 3306) from Web Server Security Group
  * Block all other inbound traffic
```

## Network ACLs 🚧

### Definition
Network Access Control Lists (ACLs) provide subnet-level traffic filtering, acting as a stateless firewall.

### Characteristics
- Controls traffic entering/leaving entire subnet
- Can control both inbound and outbound traffic.
- Stateless (checks each packet independently)
- Numbered rules processed in order
- Default: Allow all inbound and outbound traffic

### Configuration Example
```
Network ACL for Public Subnet:
- Inbound Rules:
  * Rule 100: Allow HTTP
  * Rule 200: Allow HTTPS
  * Rule 300: Block specific IP ranges
- Outbound Rules:
  * Rule 100: Allow all outbound traffic
  * Rule 200: Block specific destinations
```

## IP Addressing Fundamentals

### IP Address Structure
- IPv4 Format: Four octets (0-255)
- Example: 192.168.1.1
- Total possible addresses: 4.3 billion

## CIDR Block Notation

- Definition: CIDR (Classless Inter-Domain Routing) blocks define ranges of IP addresses. It's used to allocate and organize networks efficiently.
- Format: `<Base IP>/<Prefix Length>`
- Base IP: Starting address.
- Prefix Length: Number of fixed bits in the address (indicates network size).
- Example:
    10.0.0.0/24 includes 256 IP addresses (usable: 254).
    10.0.0.0/16 includes 65,536 IP addresses.

### CIDR Block Breakdown
- `/24` Example: 192.168.1.0/24
  * 256 IP addresses
  * First IP: Network address
  * Last IP: Broadcast address
  * Usable IPs: 254

- `/16` Example: 10.0.0.0/16
  * 65,536 IP addresses
  * Entire 10.0.x.x range

### IP Range Calculation
```
/24 CIDR Block: 192.168.1.0/24
Network Address: 192.168.1.0
First Usable IP: 192.168.1.1
Last Usable IP: 192.168.1.254
Broadcast Address: 192.168.1.255
Total Usable IPs: 254
```

## Ports and Protocols
Definition: Ports are like doors on a device where specific types of data enter and leave.

### Common Ports
- HTTP: Port 80
- HTTPS: Port 443
- SSH: Port 22
- MySQL: Port 3306
- RDP: Port 3389

### Security Group Port Configuration
When allowing HTTP (Port 80):
1. Permits inbound web traffic
2. Allows external web servers to receive requests
3. Enables communication on specific protocol

## Comparative Analysis

| Aspect | VPC | Subnets | Security Groups | Network ACLs |
|--------|-----|---------|----------------|--------------|
| Scope | Entire network | Network segment | Individual instance | Entire subnet |
| State | N/A | N/A | Stateful | Stateless |
| Rule Processing | N/A | N/A | First match allows | Processed in order |

## Best Practices
- Use multiple subnets for different environments
- Implement least privilege security groups
- Use network ACLs as an additional security layer
- Regularly audit and update network configurations
- Plan IP addressing with future growth in mind
- Separate public and private resources
- Use private IP ranges (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16)

---

## Step-by-Step Configuration Guide

### **Step 1: Configure an IP Address Range for a VPC**

When creating a Virtual Private Cloud (VPC), you must assign it an **IP address range** using a **CIDR block**. Follow these steps:

#### **What to Consider for a CIDR Block**:
1. **Size of the Network**:
   - Think about the number of resources your VPC will need.
   - For example, `/16` provides **65,536 IPs**, and `/24` provides **256 IPs**.
   - **Tip**: Plan for future growth.

2. **Private IP Address Ranges**:
   - Use private IP ranges to avoid conflicts with public IPs:
     - `10.0.0.0/8` (Large range)
     - `172.16.0.0/12` (Medium range)
     - `192.168.0.0/16` (Small range)

#### **Configuration Example**:
1. Choose `10.0.0.0/16` as your CIDR block. This allows 65,536 IP addresses.
2. During VPC creation in AWS, input this CIDR in the configuration.

---

### **Step 2: Configure Subnets within the VPC**

A subnet is a subdivision of the VPC’s CIDR block. Subnets help organize and isolate resources.

#### **What to Consider for Subnet Configuration**:
1. **Public vs. Private Subnets**:
   - **Public Subnet**: Accessible from the internet (e.g., web servers).
   - **Private Subnet**: Not directly accessible from the internet (e.g., databases).

2. **Availability Zones**:
   - Spread subnets across multiple Availability Zones for high availability.

3. **Subnet Size**:
   - Subnets require a smaller CIDR block than the VPC.
   - For example, split `10.0.0.0/16` into subnets:
     - `10.0.1.0/24` (Public Subnet)
     - `10.0.2.0/24` (Private Subnet)

#### **How to Assign IPs to Subnets**:
1. **Define Smaller CIDR Blocks**:
   - Public Subnet: `10.0.1.0/24` (256 IPs).
   - Private Subnet: `10.0.2.0/24` (256 IPs).
2. Configure the subnets in AWS and assign these CIDR blocks during the subnet creation process.

- The first 4 IPs in each subnet are reserved.

```
Example for 10.0.1.0/24:
10.0.1.0: Network address
10.0.1.1: VPC router
10.0.1.2: DNS server
10.0.1.3: Reserved for future use
10.0.1.4-254: Available for use
10.0.1.255: Broadcast address
```

---

### **Step 3: Configure Security Groups**

Security groups control access to resources within your VPC. They act as **firewalls** for individual instances.

#### **Key Features of Security Groups**:
1. **Stateful**: Responses to allowed inbound traffic are automatically allowed.
2. **Default Rules**:
   - **Inbound**: Block all traffic.
   - **Outbound**: Allow all traffic.

#### **How to Configure Rules**:
1. **Inbound Traffic Rules**:
   - Allow incoming traffic only for specific needs (e.g., HTTP, SSH).
   - Example:
     - Allow Port **80** for HTTP (web traffic).
     - Allow Port **22** for SSH (admin access).

2. **Outbound Traffic Rules**:
   - Typically allow all traffic unless there are restrictions.
   - Example:
     - Allow all outbound traffic to enable instance communication.

#### **Step-by-Step Security Group Example**:
1. Create a security group named "WebServerSG".
2. Configure the inbound rules:
   - **Allow Port 80**: For public HTTP access.
   - **Allow Port 443**: For secure HTTPS traffic.
3. Configure the outbound rules:
   - **Allow all outbound traffic**.

---

### **Step 4: Configure Ports for the Security Group**

Each rule in a security group needs a **port range**. Here’s how you decide:

#### **Port Assignment Guide**:
- **HTTP (Web traffic)**: Use **Port 80**.
- **HTTPS (Secure web traffic)**: Use **Port 443**.
- **SSH (Admin access)**: Use **Port 22**, restricted to trusted IPs.
- **Database** (e.g., MySQL): Use **Port 3306**, restricted to specific instances.

#### **Example Configuration**:
1. Add a rule to **allow inbound traffic on Port 80**:
   - Protocol: TCP
   - Port Range: 80
   - Source: Anywhere (`0.0.0.0/0` for public access).

2. Add a rule to **allow inbound traffic on Port 22**:
   - Protocol: TCP
   - Port Range: 22
   - Source: Only your IP (e.g., `192.168.1.100/32`).

3. Add a rule to **allow MySQL traffic from your app server**:
   - Protocol: TCP
   - Port Range: 3306
   - Source: Security group of your app server.

---

## Step-by-Step Configuration Process

### Step 1: Create VPC
```
AWS Console → VPC → Create VPC
- Name: production-vpc
- IPv4 CIDR: 10.0.0.0/16
- Tenancy: Default
```

### Step 2: Create Subnets
```
AWS Console → VPC → Subnets → Create Subnet
Public Subnet 1:
- Name: public-subnet-1a
- AZ: us-east-1a
- IPv4 CIDR: 10.0.1.0/24

Private Subnet 1:
- Name: private-subnet-1a
- AZ: us-east-1a
- IPv4 CIDR: 10.0.10.0/24
```

### Step 3: Create Security Groups
```
AWS Console → VPC → Security Groups → Create Security Group
Web Tier:
- Name: web-tier-sg
- Description: Web tier security group
- VPC: production-vpc
Add rules as defined above
```

### Best Practices

### VPC Design
- Always plan for growth (use larger CIDR blocks)
- Use consistent CIDR block sizes for subnets
- Document IP ranges and their purposes

### Subnet Design
- Create subnets in multiple AZs
- Use consistent naming conventions
- Leave room for additional subnets

### Security Groups
- Follow principle of least privilege
- Use security group references instead of IP ranges
- Regular audit of security group rules
- Remove unused rules
- Document purpose of each rule

### Example Production Architecture
```
VPC: 10.0.0.0/16
│
├── AZ1 (us-east-1a)
│   ├── Public: 10.0.1.0/24
│   ├── Private App: 10.0.10.0/24
│   └── Private DB: 10.0.20.0/24
│
└── AZ2 (us-east-1b)
    ├── Public: 10.0.2.0/24
    ├── Private App: 10.0.11.0/24
    └── Private DB: 10.0.21.0/24
```

---

## Security Groups vs Network ACLs in AWS

### Key Differences 🔑

| Feature | Security Groups | Network ACLs |
|---------|----------------|--------------|
| Scope | Instance level | Subnet level |
| State | Stateful | Stateless |
| Rule Processing | All rules evaluated | Rules processed in order |
| Default Behavior | Deny all inbound, Allow all outbound | Allow all inbound/outbound |
| Rule Types | Allow rules only | Allow and Deny rules |
| Association | Multiple instances | One NACL per subnet |

### Detailed Comparison

#### Security Groups 🛡️
1. **Instance Level**
   - Attached directly to EC2 instances
   - Acts as a virtual firewall for individual resources
   - Can be shared across multiple instances

2. **Stateful**
   - If inbound traffic is allowed, response traffic is automatically allowed
   - Example:
     ```
     Inbound Rule: Allow HTTP (Port 80)
     - Client sends request → Allowed by rule
     - Server response → Automatically allowed (no explicit rule needed)
     ```

3. **Rule Evaluation**
   ```
   Inbound Rules:
   - Allow HTTP (80) from 0.0.0.0/0
   - Allow HTTPS (443) from 0.0.0.0/0
   - Allow SSH (22) from 10.0.0.0/16

   All rules are evaluated before decision
   ```

#### Network ACLs 🔒
1. **Subnet Level**
   - Applied to entire subnet
   - Controls traffic entering/leaving subnet
   - One NACL per subnet

2. **Stateless**
   - Must define both inbound and outbound rules explicitly
   - Example:
     ```
     Inbound Rule: Allow HTTP (Port 80)
     Outbound Rule: Must explicitly allow response traffic
     ```

3. **Rule Processing**
   ```
   Rules processed in numerical order:
   100: Allow HTTP (80) from 0.0.0.0/0
   200: Deny traffic from 10.0.0.0/8
   300: Allow HTTPS (443) from 0.0.0.0/0
   ```

### Working Together: Security Groups and NACLs

#### Traffic Flow Example
```
Internet Request → NACL (Subnet) → Security Group (Instance) → EC2 Instance
```

1. **First Layer (NACL)**
   - Checks if traffic is allowed at subnet level
   - Processes rules in order
   - Must pass both inbound and outbound rules

2. **Second Layer (Security Group)**
   - Checks if traffic is allowed at instance level
   - All rules evaluated
   - Automatically allows return traffic

#### Practical Example

##### Scenario: Web Server Setup
```
1. VPC: 10.0.0.0/16
   └── Public Subnet: 10.0.1.0/24
       ├── NACL:
       │   Inbound:
       │   - Rule 100: Allow HTTP (80) from 0.0.0.0/0
       │   - Rule 200: Allow HTTPS (443) from 0.0.0.0/0
       │   Outbound:
       │   - Rule 100: Allow ephemeral ports (1024-65535)
       │
       └── EC2 Instance:
           └── Security Group:
               Inbound:
               - Allow HTTP (80) from 0.0.0.0/0
               - Allow HTTPS (443) from 0.0.0.0/0
```

### EC2 Instance Security Configuration

#### When Creating EC2 Instance
1. **VPC Selection**
   - Determines network isolation
   - Defines available IP range
   ```
   VPC: production-vpc (10.0.0.0/16)
   ```

2. **Subnet Selection**
   - Determines AZ placement
   - Inherits NACL rules
   ```
   Subnet: public-subnet-1a (10.0.1.0/24)
   ```

3. **Security Group Selection**
   - Instance-specific firewall rules
   - Can be modified after launch
   ```
   Security Group: web-server-sg
   ```

#### Traffic Control Flow
```
Internet Request
     ↓
VPC (Network Boundary)
     ↓
NACL (Subnet Rules)
     ↓
Security Group (Instance Rules)
     ↓
EC2 Instance
```

### Best Practices

#### Security Groups
1. **Use for Instance-Level Control**
   - Specific application requirements
   - Port-level access control
   - Instance-to-instance communication

2. **Rule Management**
   ```
   - Use service-specific groups
   - Reference other security groups
   - Regularly audit rules
   ```

#### Network ACLs
1. **Use for Subnet-Level Control**
   - Blocking specific IP ranges
   - Additional security layer
   - Broad traffic control

2. **Rule Management**
   ```
   - Keep rules ordered logically
   - Leave gaps between rule numbers
   - Document rule purposes
   ```