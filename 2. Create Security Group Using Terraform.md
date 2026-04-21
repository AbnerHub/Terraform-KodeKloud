Use Terraform to create a security group under the default VPC with the following requirements:

1) The name of the security group must be `xfusion-sg`.

2) The description must be `Security group for Nautilus App Servers`.

3) Add an inbound rule of type `HTTP`, with a port range of `80`, and source CIDR range `0.0.0.0/0`.

4) Add another inbound rule of type `SSH`, with a port range of `22`, and source CIDR range `0.0.0.0/0`.

Ensure that the security group is created in the `us-east-1` region using Terraform. The Terraform working directory is `/home/bob/terraform`. 
Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.


```
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "5.91.0"
    }
  }
}

provider "aws" {
  region                      = "us-east-1"
  skip_credentials_validation = true
  skip_requesting_account_id  = true
  s3_use_path_style = true
```


```
data "aws_vpc" "default"{
    default = true
}

resource "aws_security_group" "sg"{
    name = "xfusion-sg"
    description = "Security group for Nautilus App Servers"
    vpc_id = data.aws_vpc.default.id   
}

resource "aws_vpc_security_group_ingress_rule" "allow_http"{
    security_group_id = aws_security_group.sg.id
    cidr_ipv4 = "0.0.0.0/0"
    from_port = 80
    ip_protocol = "tcp"
    to_port = 80
}

resource "aws_vpc_security_group_ingress_rule" "allow_ssh"{
    security_group_id = aws_security_group.sg.id
    cidr_ipv4 = "0.0.0.0/0"
    from_port = 22
    ip_protocol = "tcp"
    to_port = 22
}

```


