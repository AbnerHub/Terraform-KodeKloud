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
