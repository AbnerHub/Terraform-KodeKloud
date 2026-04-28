```markdown
## 📌 Overview

1. For this task, create an AMI from an existing EC2 instance named `nautilus-ec2` using Terraform.

2. Name of the AMI should be `nautilus-ec2-ami`, make sure AMI is in available state.

3. The Terraform working directory is `/home/bob/terraform`. Update the `main.tf` file (do not create a separate .tf file) to create the AMI.


##  🚀 Solution Steps
```
# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  vpc_security_group_ids = [
    "sg-0ce2c07282148edb2"
  ]

  tags = {
    Name = "nautilus-ec2"
  }
}

# Provision ami 
resource "aws_ami_from_instance" "ami_ec2" {
  name = "nautilus-ec2-ami"
  source_instance_id = aws_instance.ec2.id
}
```

```
terraform state list 
```
Output
```
aws_ami_from_instance.ami_ec2
aws_instance.ec2
```

```
terraformm state show aws_ami_from_instance.ami_ec2
```

Output
```
# aws_ami_from_instance.ami_ec2:
resource "aws_ami_from_instance" "ami_ec2" {
    architecture         = "x86_64"
    arn                  = "arn:aws:ec2:us-east-1::image/ami-0753b596007540a7f"
    boot_mode            = "uefi"
    deprecation_time     = null
    description          = null
    ena_support          = false
    hypervisor           = null
    id                   = "ami-0753b596007540a7f"
    image_location       = null
    image_owner_alias    = null
    image_type           = "machine"
    imds_support         = null
    kernel_id            = null
    manage_ebs_snapshots = true
    name                 = "nautilus-ec2-ami"
    owner_id             = "000000000000"
    platform             = null
    platform_details     = null
    public               = false
    ramdisk_id           = null
    root_device_name     = "/dev/sda1"
    root_snapshot_id     = "snap-a60ecdfee563f7250"
    source_instance_id   = "i-37ff9214b29bb93e7"
    sriov_net_support    = null
    tags_all             = {}
    tpm_support          = null
    usage_operation      = null
    virtualization_type  = "paravirtual"

    ebs_block_device {
        delete_on_termination = false
        device_name           = "/dev/sda1"
        encrypted             = false
        iops                  = 0
        outpost_arn           = null
        snapshot_id           = "snap-a60ecdfee563f7250"
        throughput            = 0
        volume_size           = 15
        volume_type           = "standard"
    }
}
```
```
