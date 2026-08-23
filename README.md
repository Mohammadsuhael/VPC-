# EX 4 :DEPLOYMENT AND CONFIGURATION OF A PRIVATE CLOUD IN AWS
## Name : Mohammad Suhael
## Register Number : 212224230164
## Aim

To create a customised **Amazon VPC** with public and private subnets, configure route tables and security groups, and launch an **Amazon EC2 web server** inside the VPC.

## Algorithm

1. Open the AWS Management Console and select the **VPC** service.
2. Create a VPC using the **VPC and more** option.
3. Configure the VPC with CIDR block `10.0.0.0/16`.
4. Create one public subnet and one private subnet in `us-east-1a`.
5. Configure the public subnet with CIDR `10.0.0.0/24`.
6. Configure the private subnet with CIDR `10.0.1.0/24`.
7. Create an Internet Gateway and a NAT Gateway.
8. Create a second public subnet in `us-east-1b` with CIDR `10.0.2.0/24`.
9. Create a second private subnet in `us-east-1b` with CIDR `10.0.3.0/24`.
10. Associate the private subnets with the private route table.
11. Associate the public subnets with the public route table.
12. Create a security group named `Web Security Group`.
13. Allow HTTP traffic on port `80` from anywhere using IPv4.
14. Open the EC2 service and launch an instance named `Web Server 1`.
15. Select **Amazon Linux 2023** and instance type `t2.micro`.
16. Select the `vockey` key pair.
17. Configure the instance to use `lab-vpc` and `lab-subnet-public2`.
18. Enable automatic assignment of a public IP address.
19. Associate the `Web Security Group` with the EC2 instance.
20. Add the user-data script to install Apache, PHP, MariaDB and the lab web application.
21. Launch the EC2 instance.
22. Wait until the instance shows **2/2 checks passed**.
23. Copy the Public IPv4 DNS of the instance.
24. Open the Public DNS in a browser and verify that the web application is accessible.

## Program

### VPC Configuration

```text
VPC Name              : lab-vpc
IPv4 CIDR             : 10.0.0.0/16
Region                : us-east-1
Availability Zones    : us-east-1a, us-east-1b
DNS Hostnames         : Enabled
DNS Resolution        : Enabled
```

### Subnet Configuration

| Subnet                           | Type    | Availability Zone | CIDR          |
| -------------------------------- | ------- | ----------------- | ------------- |
| `lab-subnet-public1-us-east-1a`  | Public  | us-east-1a        | `10.0.0.0/24` |
| `lab-subnet-private1-us-east-1a` | Private | us-east-1a        | `10.0.1.0/24` |
| `lab-subnet-public2`             | Public  | us-east-1b        | `10.0.2.0/24` |
| `lab-subnet-private2`            | Private | us-east-1b        | `10.0.3.0/24` |

### Network Components

```text
Internet Gateway : lab-igw
NAT Gateway      : lab-nat-public1-us-east-1a

Public Route Table:
lab-rtb-public

Private Route Table:
lab-rtb-private1-us-east-1a
```

The public route table sends Internet-bound traffic through the Internet Gateway.

The private route table sends Internet-bound traffic through the NAT Gateway, allowing resources in private subnets to access the Internet without being directly accessible from the Internet.

### Security Group

```text
Security Group Name : Web Security Group
Description         : Enable HTTP access

Inbound Rule:
Protocol            : TCP
Port                : 80
Type                : HTTP
Source              : Anywhere-IPv4
Description         : Permit web requests
```

### EC2 Configuration

```text
Instance Name       : Web Server 1
AMI                 : Amazon Linux 2023
Instance Type       : t2.micro
Key Pair            : vockey
VPC                 : lab-vpc
Subnet              : lab-subnet-public2
Public IP            : Enabled
Security Group      : Web Security Group
Storage             : 8 GiB gp3
```

### User Data Script

The following user-data script was used to configure the web server automatically during instance launch:

```bash
#!/bin/bash

# Install Apache Web Server and PHP
dnf install -y httpd wget php mariadb105-server

# Download Lab files
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2/2-lab2-vpc/s3/lab-app.zip

unzip lab-app.zip -d /var/www/html/

# Turn on web server
chkconfig httpd on
service httpd start
```

## Output

### 1. VPC Created

The `lab-vpc` VPC was successfully created with CIDR block `10.0.0.0/16`.




---

### 2. Subnets Created

Four subnets were successfully created across two Availability Zones, consisting of two public and two private subnets.



---

### 3. Route Tables

The public route table was configured to route Internet traffic through the Internet Gateway, while the private route table was configured to route Internet-bound traffic through the NAT Gateway.



---

### 4. Web Security Group

The `Web Security Group` was successfully created with HTTP access on port `80`.



---

### 5. EC2 Web Server

The EC2 instance `Web Server 1` was successfully launched in the public subnet with the required security group.



---

### 6. Web Server Output


After the instance passed both status checks, its Public IPv4 DNS was opened in a browser.

The web application successfully displayed the AWS logo and EC2 instance metadata, confirming that the web server was running correctly.

## Result

The **Amazon VPC** was successfully created with public and private subnets distributed across two Availability Zones. Internet connectivity was configured using an Internet Gateway and NAT Gateway.

The `Web Security Group` was successfully configured to allow HTTP traffic, and the **EC2 Web Server 1** instance was successfully launched in the public subnet.

The web application was accessible through the instance's Public IPv4 DNS, confirming that the VPC, networking, security group, EC2 instance, and web server were configured successfully.
