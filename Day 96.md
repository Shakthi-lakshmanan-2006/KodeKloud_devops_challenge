# Day 95 – Launching an AWS EC2 Instance Using Terraform 🔧☁️

## 🚀 Title
**Day 95 of 100 Days of KodeKloud Challenge – Creating an EC2 Instance with SSH Key Pair Using Terraform**

---

## 📘 Introduction

On **Day 95**, the task focuses on using **Terraform** to provision an **AWS EC2 instance** securely using an **SSH key pair**.  
This exercise helps understand how Infrastructure as Code (IaC) works in real-world cloud automation and how Terraform manages AWS resources efficiently.

---

## 🧠 Problem Statement (Simplified)

You are required to:

- Generate an SSH key pair
- Register the public key in AWS using Terraform
- Launch an EC2 instance
- Attach the created key pair and default security group
- Verify the setup using Terraform commands

In short: **Create a secure EC2 instance using Terraform and SSH authentication.**

---
![Screenshot](./assets/Screenshot%202026-01-03%20194410.png)

---

## 📚 Prerequisites / Things to Know

Before starting this task, you should have basic knowledge of:

- AWS EC2 concepts
- Terraform workflow:
  - `init`
  - `validate`
  - `plan`
  - `apply`
- SSH key pairs
- Terraform resource & data blocks

---

## 🛠️ What This Task Demonstrates

- Using `aws_key_pair` resource in Terraform
- Referencing existing AWS resources using `data` blocks
- Linking Terraform-managed resources together
- End-to-end EC2 provisioning using IaC

---

## 🧩 Steps I Followed to Complete the Task

### 1️⃣ Generate SSH Key Pair

```bash
ssh-keygen -t rsa -f xfusion-kp
```

This creates:
- `xfusion-kp` (private key)
- `xfusion-kp.pub` (public key)

---

### 2️⃣ Initialize Terraform

```bash
terraform init
```

- Downloads AWS provider
- Prepares Terraform backend

---

### 3️⃣ Validate Configuration

```bash
terraform validate
```

- Ensures Terraform files are syntactically correct

---

### 4️⃣ Apply Terraform Configuration

```bash
terraform apply
```

- Creates AWS Key Pair
- Launches EC2 instance
- Attaches default security group
- Uses generated SSH key for access

---

## 📄 Terraform Configuration (main.tf)

```hcl
resource "aws_instance" "xfusion_ec2" {
  ami                    = "ami-0c101f26f147fa7fd"
  instance_type          = "t2.micro"
  vpc_security_group_ids = [data.aws_security_group.default.id]
  key_name               = aws_key_pair.xfusion_kp.key_name

  tags = {
    Name = "xfusion-ec2"
  }
}

resource "aws_key_pair" "xfusion_kp" {
  key_name   = "xfusion-kp"
  public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCUICJxbiWvq2AjYaOZtDKrt7MDceUmhX+S3hJDEEEHhtKId7mDa2iHVL4yaGI9pzXXgC7n4IVk8lAVUgcvwXj/ueFx4VL+rO6nw3Vd5pY6IEtdcPmnhb0kqmiti3pRy2szlko2W3Q8UcJ5AtL0tj2PkfK6MNSB+eDgbi0PqXOb6KDcsbaaudqbHdM+zTTu4E2aUSZBoaTU8oTd08dl2klOoA8sNq07sKz7Kzn+O9284kVWKN7UN+aE3BIPuPTrTljzqD3UFqBOlYZaxcHR96gGW2PypVsd6EvlssentZV2o+y5xJey+UJfSXpenIMXd/29fE7jGHem3Oy49d4LY/HgLVugoaZzuXUmroEdujJ+CZR4ux6aV1wb7jp1R6thlMv9UG4dHRYOExe1UEfHJoAZxSTiNV8tkcQ/uHqbmJXSWk7jgE0PW6TVJ0nmGZirC4/lJ40jnEQTfD/6xl1ZSc9t7dtJZjRC0W3Kx2otnWaGJo/khe6EI2qdTWCfXhUjBzM= bob@iac-server"
}

data "aws_security_group" "default" {
  name = "default"
}
```

---
![Screenshot](./assets/Screenshot%202026-01-03%20195928.png)

---

## 📌 Summary of Commands Used

| Command | Purpose |
|------|--------|
| `ssh-keygen` | Generate SSH key pair |
| `terraform init` | Initialize Terraform |
| `terraform validate` | Validate configuration |
| `terraform apply` | Create AWS resources |

---

## ✅ Outcome

- EC2 instance successfully created
- SSH key pair registered in AWS
- Infrastructure managed completely via Terraform
- Task completed as part of **Day 95 – KodeKloud 100 Days Challenge** 🎯

---
![Screenshot](./assets/Screenshot%202026-01-03%20200303.png)

---

✨ *Another step forward in mastering Terraform and cloud automation!* ✨
