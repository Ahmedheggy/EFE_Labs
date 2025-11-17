# Route 53 Private Hosted Zone Setup (heggy.com)
![Architecture Diagram](img/result.png)

## Overview

This guide describes how to create a Route 53 Private Hosted Zone using
the domain **heggy.com**, and how to create two DNS A-records pointing
to two EC2 instances inside the same VPC.\
It also includes the required Security Group configuration to allow SSH
and ICMP (ping).

------------------------------------------------------------------------

## 1. Create a Private Hosted Zone

1.  Open the AWS Console\
2.  Go to **Route 53 → Hosted Zones**\
3.  Click **Create hosted zone**\
4.  Domain name:\
    `heggy.com`\
5.  Type:\
    `Private hosted zone for Amazon VPC`\
6.  Select your VPC\
7.  Click **Create hosted zone**

------------------------------------------------------------------------

## 2. Create DNS A-Records

### Record A (First EC2)

-   **Record name:** `a.heggy.com`\
-   **Record type:** A\
-   **Value:** `10.0.4.35`

### Record B (Second EC2)

-   **Record name:** `b.heggy.com`\
-   **Record type:** A\
-   **Value:** `10.0.10.62`

These records resolve only inside the VPC.

------------------------------------------------------------------------

## 3. Configure EC2 Security Group

Attach the same Security Group to both EC2 instances, with the following
inbound rules:

### Inbound Rules

  --------------------------------------------------------------------------
  Type          Protocol   Port   Source               Purpose
  ------------- ---------- ------ -------------------- ---------------------
  SSH           TCP        22     Your Public IP       Allow SSH access

  All ICMP      ICMP       All    VPC CIDR             Allow ping inside VPC
                                  (10.0.0.0/16)        

  All Traffic   All        All    Security Group ID    EC2-to-EC2 internal
                                                       communication
  --------------------------------------------------------------------------

### Outbound Rules

Keep default:\
`All traffic → 0.0.0.0/0`

------------------------------------------------------------------------

## 4. Result

-  EC2 instance A can reach EC2 B with the assigned Domain in route53 host & vice versa

    ping a.heggy.com
    ping b.heggy.com


------------------------------------------------------------------------

## Summary

-   Domain used: **heggy.com**\
-   A-records created:
    -   `a.heggy.com → 10.0.4.35`\
    -   `b.heggy.com → 10.0.10.62`\
-   Security Group updated to allow SSH and ICMP\
-   EC2s in same VPC can resolve via Route 53 Private DNS

------------------------------------------------------------------------
