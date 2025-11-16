Deploying EC2 Instances Behind an Application Load Balancer

Introduction

load balancing infrastructure was set up on AWS using a VPC, EC2 Instances, and an Application Load Balancer (ALB). The goal was to distribute traffic evenly across EC2 Instances located in Multiple Availability Zones (AZs) to ensure high availability and fault tolerance.

Steps Taken

1. Creating the VPC

A new VPC was created to act as the isolated network environment hosting all resources.

The VPC was configured with Subnets in Multiple Availability Zones (AZs).

Route Tables were set up to enable communication between Subnets.

2. Setting Up Subnets

Public Subnets were created in two AZs:

Public Subnet in AZ-1.

Public Subnet in AZ-2.

Route Tables were configured to allow internet access via Internet Gateway.

3. Creating Security Groups

Security Group for ALB:

Configured to allow inbound traffic on Port 80 (HTTP) from all sources (0.0.0.0/0).

Security Group for EC2 Instances:

Configured to allow inbound traffic on Port 80 (HTTP) only from the ALB Security Group.

Added an SSH rule (Port 22) allowing access from my IP (My IP only).

4. Setting Up EC2 Instances

EC2 Instances were launched in two Availability Zones.

Apache or Nginx was installed on each EC2 Instance to serve static content over HTTP:

EC2 Instance A in AZ-1.

EC2 Instance B in AZ-2.

5. Creating the Application Load Balancer (ALB)

An Application Load Balancer (ALB) was created to distribute traffic to the EC2 Instances in Multiple Availability Zones.

A Listener was configured for Port 80 (HTTP).

The ALB was linked to a Target Group that includes EC2 Instances A and B.

Health checks were configured to ensure that only healthy EC2 Instances receive traffic.

6. Creating the Target Group for EC2 Instances

EC2 Instances were added to a Target Group.

Health Checks were configured for the Target Group to check the health of the instances.

The Health Check was set to HTTP with a Path of / on Port 80.

7. Testing Traffic Distribution

The ALB URL was tested in the browser to ensure traffic is correctly routed to EC2 Instance A and EC2 Instance B.

Verified that the ALB distributes traffic equally between the two EC2 Instances.

8. Final Submission

Final tests were conducted to ensure traffic distribution was functioning properly.

Verified that Health Checks were passing and the EC2 Instances were functioning correctly.

Results:

The Load Balancer was successfully configured to distribute traffic across EC2 Instances located in Multiple Availability Zones.

The application was made fault-tolerant, ensuring it remains available even if one Availability Zone fails.

Verified that traffic is routed only to healthy EC2 Instances.

Challenges and Solutions

Health Check Issue: Encountered some challenges configuring the Health Check correctly. This was resolved by ensuring the Path in the Health Check pointed to the correct endpoint on the EC2 Instances.
