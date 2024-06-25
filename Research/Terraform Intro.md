---
Author(s): M-Nelly
Source: Internal
Subject: Terraform
Topic: Automation
Description: 
Comments: 
tags:
---
## To-Do
- [ ] 
## Notes
To get started, you'll need to specify a provider: 
https://registry.terraform.io/browse/providers
- specify versions for the provider and Terraform Version
```json
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "5.52.0"
    }
  }
  required_version = ">= 1.8.4"
}

provider "aws" {
  # Configuration options
  region = "us-east-1"
}
```
In the example repository, we then tell AWS to create a VPC:
```json
resource "aws_vpc" "example" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "dev-vpc"
  }
}
```
We then create an internet gateway:
```json
resource "aws_internet_gateway" "example" {
  vpc_id = aws_vpc.example.id
}
```
And then we create a subnet:
```json
resource "aws_subnet" "example" {
  vpc_id            = aws_vpc.example.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "us-east-1a"
  tags = {
    Name = "dev-subnet"
  }
}
```
So far we have labeled these resources with "example". This could be anything. It just gives us a name to call the resource by. You can see this in action in the `vpc_id = aws_vpc.example.id` field. The tags that we have set are the names we can search for in the AWS GUI. 

Now we need to add a route table and a default route:
```json
resource "aws_route_table" "example" {
  vpc_id = aws_vpc.example.id

  tags = {
    Name = "example-route-table"
  }
}

resource "aws_route" "default_route" {
  route_table_id         = aws_route_table.example.id
  destination_cidr_block = "0.0.0.0/0"
  gateway_id             = aws_internet_gateway.example.id
}

resource "aws_route_table_association" "example" {
  subnet_id      = aws_subnet.example.id
  route_table_id = aws_route_table.example.id
}
```
In the next section we define an AWS security group that will effectively define firewall rules for our VPC:
```json
resource "aws_security_group" "example" {
  name_prefix = "dev-ssh-access"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["66.0.0.97/32"] 
  }
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["66.0.0.97/32"] 
  }
  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["66.0.0.97/32"] 
  }
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"] 
  }
  tags = {
    Name = "dev-security-group"
  }

  vpc_id = aws_vpc.example.id
}
```
Now we want to create an EC2 instance and configure it with some commands:
```json
resource "aws_instance" "example" {
  ami           = "ami-03a6eaae9938c858c" 
  instance_type = "t2.micro"              
  key_name      = "dev-example-key"       
  subnet_id     = aws_subnet.example.id   
  tags = {
    Name = "dev-amazon"
  }

  root_block_device {
    volume_size = 30    
    volume_type = "gp3"
    encrypted   = true
  }
  associate_public_ip_address = true
  vpc_security_group_ids = [
    aws_security_group.example.id 
  ]
  user_data = <<EOF
#!/bin/bash
sudo dnf --assumeyes update
sudo dnf --assumeyes upgrade
sudo dnf install -y epel-release

sudo dnf --assumeyes install firewalld
sudo systemctl start firewalld
sudo systemctl enable firewalld
sudo firewall-cmd --zone=public --permanent --add-service=ssh
sudo firewall-cmd --zone=public --permanent --add-service=http
sudo firewall-cmd --zone=public --permanent --add-service=https
sudo systemctl reload firewalld

sudo dnf module enable nginx:mainline
sudo dnf --assumeyes install nginx
sudo systemctl enable --now nginx

EOF
}
output "ec2_global_ips" {
  value = aws_instance.example.public_ip
}
```
I have truncated the script for the sake of readability, but you should be writing a script to include in here to get your machine into a working state. 

Once you have a working configuration, you will need to run some Terraform commands to get up and running.
```sh
terraform validate #checks validity of config file
terraform fmt #formats your code 
terraform plan #show changes required by current config
terraform apply #create or update infrastructure
terraform destroy #destroy previously created infrastructure
```

## References
### External
- https://github.com/towerindustries/terraform_training/blob/main/phase_1/019_complete_code/main.tf
### Internal
- 