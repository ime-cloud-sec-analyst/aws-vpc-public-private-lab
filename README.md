# aws-vpc-public-private-lab
Hands-on lab: Build a custom AWS VPC with public &amp; private subnets, IGW, NAT Gateway, Bastion host, and private EC2 instance.
AWS VPC Public-Private Lab

This repository contains all artifacts and documentation for the hands-on AWS lab where we built a custom Virtual Private Cloud (VPC) with both public and private subnets, configured routing (Internet Gateway & NAT Gateway), deployed a bastion host in the public subnet, and launched a private EC2 instance accessible only via the bastion host.

Lab Overview

Objective:

Create a VPC with a /16 CIDR block (10.0.0.0/16) named Lab VPC.

Enable DNS hostnames and DNS resolution.

Create two subnets in one AZ (us-west-2a):

Public Subnet (10.0.0.0/24) with auto-assign public IPv4.

Private Subnet (10.0.2.0/23) without public IP assignment.

Attach an Internet Gateway (IGW) to allow outbound/inbound internet traffic for public subnet.

Create two route tables:

Public Route Table (not main) with default route (0.0.0.0/0) → IGW, associate with Public Subnet.

Private Route Table (main) with default route (0.0.0.0/0) → NAT Gateway, associate with Private Subnet.

Allocate an Elastic IP (EIP) and create a NAT Gateway in the Public Subnet.

Deploy a Bastion Host EC2 instance in the Public Subnet with SSH access from your IP.

Deploy a Private EC2 instance in the Private Subnet (no public IP).

Configure Security Groups:

Bastion SG: Allow SSH (TCP 22) from your office IP.

Private Instance SG: Allow SSH (TCP 22) only from Bastion SG.

Validate connectivity:

SSH into Bastion.

From Bastion, SSH into Private instance via its private IP.

Architecture Diagram

+----------------------+        +-------------+
|      Internet        |        | Elastic IP  |
+----------+-----------+        +------+------+   +------------------+
           |                         | IGW  |      | NAT Gateway      |
           |                         +--+---+      +--------+---------+
           |                            |               |
      +----v----+                  +----v----+     +----v----+
      | Bastion |                  | Public  |     | Private |
      |  Host   |                  | Subnet  |     | Subnet  |
      | (Public)|                  | (10.0.0/24)   | (10.0.2/23)|
      +----+----+                  +----+----+     +----+----+
           |                             |               |
           | SSH session                 | SSH traffic   |
           +----------> EC2 (Public) <---+   routed via  |
                         SSH SG                 NAT Gateway

File Structure

/README.md           # This file
/screenshots/        # All lab screenshots, numbered by step
  01-vpc-created.png
  02-enable-dns.png
  ...
  23-nat-created.png

Usage

Clone this repository:

git clone https://github.com/ime-cloud-sec-analyst/aws-vpc-public-private-lab.git

Review the lab steps in README.md and corresponding screenshots in /screenshots/.

Launch AWS resources manually or via IaC (CloudFormation/Terraform).

Validate connectivity:

SSH to Bastion using its public IP.

From Bastion, SSH to Private instance via its private IP.

Conclusion

This lab demonstrates how to securely segment AWS workloads into public and private subnets, leverage a bastion host for secure administrative access, and use a NAT Gateway for controlled outbound internet access from private resources.

License

This project is licensed under the MIT License
