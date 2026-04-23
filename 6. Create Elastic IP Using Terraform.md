## 📌 Tasks

For this task, allocate an Elastic IP address named xfusion-eip using Terraform.

The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.

## 🚀 Steps solution

```
resource "aws_eip" "xfusion-eip"{
    tags = {
        Name = "xfusion-eip"
    }
}
```

## ✅ Verification 

To list the resources created we use:

```
terraform state list
```
To show the elastic Ip created, we use 

```
terraform state show aws_ipx.fusion-eip | grep public_ip
```

Output

```
ep  public_ip
    public_ip                = "127.189.219.148"
    public_ipv4_pool         = null

```

## 🔍 Links 

https://registry.terraform.io/providers/-/aws/6.8.0/docs/resources/eip
