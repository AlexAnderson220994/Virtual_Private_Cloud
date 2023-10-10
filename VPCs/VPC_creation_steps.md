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

![Alt text](<../images/vpc create.jpg>)

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
7) Click `Create Subnet`.
![Alt text](<../images/public subnet.jpg>)

### 3) Creating Internet Gateway

1) On the AWS console, go to the VPC page.
2) On the left hand pane, under "Virtual Private Cloud", Click on `Internet gateways`.
3) In the top right hand corner, click on `Create Internet gateway`.
4) Name the gateway. e.g. `tech254-alex-2tier-second-vpc-ig`.
5) Click `Create Internet gateway`.
6) On the "Details" page of the Internet gateway you just made, go to `Actions`, then click `Attach to VPC`.
7) Search for the VPC you made and click it.
8) Click `Attach Internet gateway`.
![Alt text](<../images/internet gateway.jpg>)

### 3) Creating Route Table

1) On the AWS console, go to the VPC page.
2) On the left hand pane, under "Virtual Private Cloud", Click on `Route tables`.
3) In the top right hand corner, click on `Create route table`.
4) Name it `public-rt`.
5) From the VPC dropdown, select the VPC you made.
6) Click `Create route table`.
![Alt text](<../images/route table.jpg>)

### 4) Subnet associations

1) On the details page for the Route table you just made, click on the "Subnet associations" tab.
![Alt text](<../images/route table details.jpg>)
2) Under "Explicit subnet associations", click on `Edit subnet associations`.
3) Click the tick box next to `public-subnet`.
4) Click `Save associations`.

### 5) Editing Route table

1) On the details page for the Route table you just made, click on the "Route" tab.
![Alt text](<../images/Screenshot 2023-10-10 092041.jpg>)
2) Click on `Edit routes`.
3) Click on `Add route`:
- For "Destination", choose `0.0.0.0/0`
- For "Target", choose `Internet Gateway`, then select the Internet gateway you made from the dropdown list that follows.

### 6) Check the VPC pathway

1) Go onto your VPC details page and check the resource map is following the correct path for each subnet.
![Alt text](<../images/Screenshot 2023-10-10 092826.jpg>)

### 7) Make a database instance

1) Go into EC2 and go to the launch instance page OR go to AMIs and choose the AMI you made for the database.
2) Name your instance. e.g. `tech254_alex_db_test_second_vpc`.
3) Keep the same instance type and key pair login you normally use.
4) Under "Network Settings":
- Select `edit`
- Under "VPC", choose your VPC you made earlier
- Under "Subnet", choose the `private-subnet` you made earlier
- **DISABLE** "Auto-assign public IP"
![Alt text](<../images/network settings db instance.jpg>)
5) For the "Security group":
- Create (or use previously made) security group to allow the following:
- SSH, Port 22, anywhere
- MongoDB- Port 27017, anywhere
6) Click `Launch Instance`.

### 8) Make an app instance

1) Go into EC2 and go to the launch instance page OR go to AMIs and choose the AMI you made for the database.
2) Name your instance. e.g. `tech254_alex_app_test_second_vpc`.
3) Keep the same instance type and key pair login you normally use.
4) Under "Network Settings":
- Select `edit`
- Under "VPC", choose your VPC you made earlier
- Under "Subnet", choose the `public-subnet` you made earlier
- **ENABLE** "Auto-assign public IP"
5) For the "Security group":
- Create (or use previously made) security group to allow the following:
- SSH, Port 22, anywhere
- HTTP, Port 80, anywhere
- App, Port 300, anywhere
6) Add the user data to seed the database and run the app:
````
#!/bin/bash

export DB_HOST=mongodb://<public/private_IP_of_DB_instance>:27017/posts

cd /home/ubuntu/repo/app
sudo systemctl restart nginx
npm install

node seeds/seed.js

sudo npm install pm2 -g
pm2 kill
pm2 start app.js
````
- Make sure to change <public/private_IP_of_DB_instance> to the private IP of your database
![Alt text](<../images/app user data.jpg>)
7) Click `Launch Instance`.

## Cleaning up

- Go to your VPC details and click on `Delete VPC`.
- All associations will be deleted alongside this.