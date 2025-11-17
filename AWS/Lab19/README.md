# Cross-Region EBS Backup and Recovery Lab

This lab demonstrates how Amazon EC2 and EBS work together to provide regional resiliency, backup capability, and cross-region data recovery. Students will simulate backing up application data from one region and restoring it in another.

## Region A – Source Environment

### EC2 Instance
- Runs Amazon Linux or Ubuntu.
- Lightweight instance type (t2.micro or t3.micro).
- Operates inside the default VPC with default networking.

### Storage Layout
- **Root volume**: Automatically created with the instance.
- **Extra EBS data volume**: 10–20 GB (gp3) used for application data or custom files.

### Snapshot Requirements
- Snapshot must be created from the extra EBS volume only (not root volume).
- Must allow cross-region copying.
- Must include tags:
  - Original region
  - Purpose (e.g., backup, disaster recovery, migration)

## Region B – Recovery Environment

### Snapshot Copy
- The snapshot from Region A must be successfully copied to Region B.
- Tags from Region A must be retained.

### Volume Creation
- Create a new EBS volume from the snapshot copy.
- Place the volume in the same AZ as the target EC2 instance.

### EC2 Instance
- Launch a new EC2 instance in the same AZ as the restored volume.
- Simulate a disaster-recovery or migration environment.

### Volume Attachment
- Attach the restored EBS volume to the new EC2 instance as a secondary data volume.
- Verify that data from Region A can be accessed in Region B.

## Goal
Demonstrate how data persists across regions and how to recreate compute environments from previously backed-up EBS volumes.
