# VPC Creation

## VPC, Subnet, Internet Gateway and Route Table creation process

### 1) Creating a VPC

1) On the AWS console, go to the VPC page.
2) On the left hand pane, under "Virtual Private Cloud", Click on `Your VPCs`.
3) In the top right hand corner, click on `Create VPC`.
4) Click on `VPC only`.
5) Give your VPC a name e.g.
````
tech254-alex-2tier-first-vpc
````
6) Set IPv4 CIDR as:
````
10.0.0.0/16
````
7) Click `Create VPC`.

### 2) Creating Subnets

1) On the AWS console, go to the VPC page.
2) On the left hand pane, under "Virtual Private Cloud", Click on `Subnets`.
3) In the top right hand corner, click on `Create Subnet`.
4) Under "VPC ID", search for the VPC you created.
5) For subnet 1 of 2: 
- Name it `public-subnet`.
- Put it in "Availability Zone" `eu-west-1a`.
- Put "IPv4 subnet CIDR block" as `10.0.2.0/24`.
6) For subnet 2 of 2: 
- Name it `private-subnet`.
- Put it in "Availability Zone" `eu-west-1b`.
- Put "IPv4 subnet CIDR block" as `10.0.3.0/24`.

###) Creating


## Cleaning up

