# Day 95 – Terraform: Create a Security Group in Default VPC (AWS)

## 📌 Challenge Title
**Creating an AWS Security Group using Terraform in the Default VPC**

---

## 🧩 Introduction

On **Day 95** of my **100 Days of KodeKloud Challenge**, I worked on a Terraform task focused on AWS networking.  
The objective was to create an **AWS Security Group** in the **default VPC** using Terraform with specific inbound rules.

This task helped reinforce how Terraform interacts with existing AWS resources (like the default VPC) using **data sources**, and how to define security rules declaratively.

---

## 🔍 Problem Statement (Simplified)

Using **Terraform**, create a security group with the following requirements:

- Security Group Name: `nautilus-sg`
- Description: `Security group for Nautilus App Servers`
- Must be created in the **default VPC**
- Region: **us-east-1**
- Inbound Rules:
  - Allow **HTTP** traffic on port **80** from `0.0.0.0/0`
  - Allow **SSH** traffic on port **22** from `0.0.0.0/0`
- Outbound Rule:
  - Allow all traffic
- File to be used: `main.tf` (do not create additional `.tf` files)

---

## 📚 Prerequisites / Things to Know

Before starting this task, it’s important to understand:

- **Terraform basics**
  - `terraform init`
  - `terraform validate`
  - `terraform plan`
  - `terraform apply`
- Difference between:
  - **Resources** (create/manage infrastructure)
  - **Data sources** (read existing infrastructure)
- AWS concepts:
  - What is a **Security Group**
  - What is a **VPC** (especially the default VPC)
- Terraform AWS Provider configuration

---

## 🛠️ Approach & Explanation

Instead of creating a new VPC, I used a **data source** to fetch the existing **default VPC**.  
This avoids unnecessary resource creation and follows best practices.

The security group:
- Allows HTTP and SSH inbound traffic
- Allows all outbound traffic
- Is explicitly attached to the default VPC

---

## 🧾 main.tf File Content

```hcl
resource "aws_security_group" "nautilus_sg" {
  name        = "nautilus-sg"
  description = "Security group for Nautilus App Servers"
  vpc_id      = data.aws_vpc.default.id

  tags = {
    Name = "nautilus-sg"
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
    description = "Allow HTTP traffic"
  }

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
    description = "Allow SSH traffic"
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

data "aws_vpc" "default" {
  default = true
}
```

---

## ▶️ Steps I Followed to Complete the Task

1. Initialized Terraform:
   ```bash
   terraform init
   ```

2. Validated the configuration:
   ```bash
   terraform validate
   ```

3. Reviewed the execution plan:
   ```bash
   terraform plan
   ```

4. Applied the configuration:
   ```bash
   terraform apply
   ```

5. Confirmed that the security group was created successfully in AWS.

---

## 🧮 Summary of Commands Used

| Command | Purpose |
|------|--------|
| `terraform init` | Initialize Terraform and download providers |
| `terraform validate` | Validate Terraform configuration |
| `terraform plan` | Preview infrastructure changes |
| `terraform apply` | Create the security group |

---

## ✅ Key Takeaways

- Use **data sources** to reference existing infrastructure
- Always validate and plan before applying changes
- Terraform makes infrastructure creation **repeatable and reliable**
- Security groups are stateful and critical for AWS networking

---

🚀 **Day 95 completed successfully!**  
Onward to the next challenge in the **100 Days of KodeKloud** journey.
