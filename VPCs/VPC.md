# VPC - Virtual Private Clouds

- AWS use VPCs
- Azure uses VNs (Virtual Network)
- On Azure theres no such thing as default VPCs/VNs - each one has to be set up from scratch.
- Means more set up time is required with Azure.

CIDR Block - Range of IP Addresses
10.0.0.0/16

- NACL - Network Access Control List
- Essentially a security group for the subnet

## VPC Creation Steps

1) Create a VPC
2) Make two Subnets (Public and Private)
3a) Create Internet Gateway
3b) Attach Internet Gateway to VPC
4) Make a Public Route Table
5) Associate Route Table to the Public Subnet
