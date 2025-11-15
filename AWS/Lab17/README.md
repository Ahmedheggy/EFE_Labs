# Multi-VPC AWS Lab - README

## Objective
Design, deploy, and document a multi-VPC network architecture on AWS using a Transit Gateway to interconnect three isolated tiers—Public (Bastion + NAT), Application (App Server), and Database (DB Server)—while enforcing best practices for routing, segmentation, controlled internet access, and secure east-west communication.

---

## Architecture Components

### 1) VPC A (Public Tier)
- **Role:** Acts as the public entry point and outbound internet provider.
- **Components:**
  - NAT Gateway for outbound internet access from private VPCs.
  - Bastion Host for secure SSH/administrative access.
- **Connectivity:**
  - Attached to the Internet Gateway (IGW).
  - Connected to the Transit Gateway (TGW) for internal traffic.
- **Exposure:**
  - Public subnet for Bastion and NAT.

### 2) VPC B (Application Tier)
- **Role:** Hosts the application server providing business logic or API functionality.
- **Connectivity:**
  - Communicates with VPC C (DB) strictly via Transit Gateway.
  - Routes all outbound internet traffic through VPC A’s NAT Gateway.
- **Security:**
  - No direct internet connectivity.
  - Access limited to Bastion Host and DB tier as needed.

### 3) VPC C (Database Tier)
- **Role:** Contains the database layer used only by the application server.
- **Exposure:** Internal use only—no public access.
- **Connectivity:** App Server in VPC B is the only allowed consumer via Transit Gateway.
- **Security:** Highly restricted security groups; no inbound internet routes.

### Transit Gateway (TGW)
- **Purpose:** Central routing hub allowing scalable VPC-to-VPC communication.
- **Configuration:**
  - Attachments for VPC A, VPC B, and VPC C.
  - Route tables configured to permit App → DB communication while maintaining segmentation.
  - East-west traffic isolated from the internet.

---

## Deployment Steps

1. **VPC Creation**
   - Provision three VPCs with the following CIDRs:
     - VPC A (Public): 10.0.0.0/16
     - VPC B (App): 172.16.0.0/16
     - VPC C (DB): 192.168.0.0/16
   - Create subnets (public/private) and route tables.

2. **IGW, NAT, and Public Access**
   - Attach an Internet Gateway to VPC A.
   - Deploy NAT Gateway in VPC A public subnet.
   - Configure routing so that VPC B and VPC C use NAT via Transit Gateway for outbound internet.

3. **Transit Gateway Integration**
   - Attach all VPCs to the Transit Gateway.
   - Configure route table propagation and association.
   - Ensure:
     - App Tier can reach DB Tier.
     - Bastion Host can reach both private VPCs.
     - No unintended cross-tier exposure.

4. **Database Tier**
   - Deploy DB Server in VPC C private subnet.
   - Restrict inbound access solely to App Server IP (172.16.141.50/32) and Bastion if needed for admin.
   - Ensure no direct NAT/IGW access.

5. **Application Tier**
   - Deploy App Server in VPC B private subnet.
   - Configure routing for:
     - Outbound → NAT Gateway in VPC A (0.0.0.0/0 outbound traffic allowed).
     - East-west → DB Server via Transit Gateway.
   - Security groups:
     - Allow SSH from Bastion Host (10.0.6.202/32).
     - Allow MySQL/TCP 3306 to DB Server (192.168.0.0/16).

6. **Bastion Host & Administrative Access**
   - Deploy Bastion Host in VPC A public subnet.
   - Allow controlled SSH access to App and DB servers.
   - Security groups inbound example:
     - SSH from your public IP to Bastion.
     - App and DB allow inbound SSH only from Bastion internal IP (10.0.6.202/32).

7. **Security Groups**
   - **Bastion:**
     - Inbound: SSH from your IP.
     - Outbound: All traffic (0.0.0.0/0).
   - **App Server:**
     - Inbound: SSH from Bastion (10.0.6.202/32), MySQL if needed.
     - Outbound: All traffic through NAT Gateway (0.0.0.0/0).
   - **DB Server:**
     - Inbound: MySQL/TCP 3306 from App Server IP (172.16.141.50/32), SSH from Bastion if needed.
     - Outbound: restricted or none, no direct internet.

8. **Testing**
   - SSH from local machine to Bastion.
   - From Bastion, SSH to App Server.
   - From App Server, connect to DB (MySQL or ping if enabled).
   - Validate outbound internet access from App via NAT Gateway.
   - Ensure DB cannot access internet directly.

9. **Deliverables (Screenshots)**
   - VPCs, subnets, and route tables.
   - Transit Gateway attachments and route tables.
   - Bastion, App, and DB EC2 instances running.
   - Security groups showing allowed paths.
   - NAT Gateway and IGW setup.

---

**Notes:**
- Outbound rules of App Server left as 0.0.0.0/0 to allow NAT Gateway usage.
- DB Server inbound limited strictly to App Server internal IP (172.16.141.50/32).
- Bastion Host used for controlled SSH access to App and DB.
- Transit Gateway attachments configured to enable east-west traffic while maintaining segmentation.
- Security group internal IPs used for least-privilege access.

