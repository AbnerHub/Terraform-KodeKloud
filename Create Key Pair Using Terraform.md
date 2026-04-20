## 📌 Create Key Pair Using Terraform

The **Nautilus DevOps team** is strategizing the migration of a portion of their infrastructure to the **AWS cloud**. 
Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.
To achieve this, they have segmented large tasks into smaller, more manageable units.

This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations.
By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, 
risk mitigation, and optimization of resources throughout the migration process.


**Requirements:**
- Name of the key pair should be `xfusion-kp`
- Key pair type must be `rsa`
- The private key file should be saved under `/home/bob/xfusion-kp.pem`
- The Terraform working directory is `/home/bob/terraform`
- Create the `main.tf` file (do not create a different .tf file)


## 🚀 Solution steps 

1. In the `aws-client` navigate to `/home/bob/terraform` directory
2. create a new ``main.tf` file

```
vi main.tf
``` 

3. Into the directory write the next terraform code lines.

```
resource "tls_private_key" "rsa_key"{
    algorithm = "RSA"
    rsa_bits = "4096"
}  

resource "aws_key_pair" "key_pair"{
    key_name = "xfusion-kp"
    public_key = tls_private_key.rsa_key.public_key_openssh
}

resource "local_sensitive_file" "private_key"{
    filename = "/home/bob/xfusion-kp.pem"
    content = tls_private_key.rsa_key.private_key_pem
    file_permission = "0600"
}
```

**Notes**

If the `.pub` key is already created and we don´t need to create it with `tls_private_key`

Into  `aws_key_pair` resource:

```
public_key = file (".ssh/<.pub key name>") # there is the .pub path file
```

Once created and saved the `main.tf` file. Deploy the infraestruture with the next terraform  commands.

```
terraform init 
```

```
terraform apply

```

**Expected Output** 

```
Terraform will perform the following actions:

  # aws_key_pair.xfusion_kp will be created
  + resource "aws_key_pair" "xfusion_kp" {
      + key_name     = "xfusion-kp"
      + key_type     = "rsa"
      + public_key   = (known after apply)
    }

  # local_file.private_key_pem will be created
  + resource "local_file" "private_key_pem" {
      + content         = (sensitive value)
      + filename        = "/home/bob/xfusion-kp.pem"
      + file_permission = "0600"
    }

  # tls_private_key.xfusion_key will be created
  + resource "tls_private_key" "xfusion_key" {
      + algorithm   = "RSA"
      + rsa_bits    = 4096
    }

Plan: 3 to add, 0 to change, 0 to destroy.

Apply complete! Resources: 3 added, 0 changed, 0 destroyed.
```
   



    
