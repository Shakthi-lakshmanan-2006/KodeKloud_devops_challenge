# Day 94 — Creating an AWS VPC using Terraform 🚀

## 📌 Title
**Day 94: Provisioning an AWS VPC with Terraform (KodeKloud Engineer Lab)**

---

## 🧩 Introduction

On **Day 94** of my **100 Days KodeKloud Challenge**, the task focused on using **Terraform** to create an **AWS VPC**.  
This lab helps strengthen the fundamentals of **Infrastructure as Code (IaC)** by provisioning cloud resources in a clean, repeatable, and automated way.

The objective was simple but important:  
👉 Create a VPC named **xfusion-vpc** in the **us-east-1** region using Terraform.

---

## 🧠 Understanding the Problem (In Simple Terms)

Instead of manually creating a VPC from the AWS Console, we use **Terraform** to define the infrastructure in code.  
Terraform then:
- Validates the configuration
- Shows what will be created
- Applies the configuration to AWS

This approach ensures:
- Consistency
- Automation
- Version control for infrastructure

---

## 📚 Prerequisites / Things to Know Before Starting

Before working on this task, it helps to know:

- Basics of **AWS VPC**
  - What a VPC is
  - CIDR blocks (IPv4)
- Terraform fundamentals:
  - `provider`
  - `resource`
  - `terraform init`
  - `terraform plan`
  - `terraform apply`
- AWS provider configuration in Terraform
- Working with files like `main.tf`

---

## 🛠️ Task Requirements

- Create a VPC named **xfusion-vpc**
- Region: **us-east-1**
- CIDR block: Any valid **IPv4 CIDR**
- Terraform working directory:  
  `/home/bob/terraform`
- Use **only `main.tf`** (do not create extra `.tf` files)

---

## 🧪 Steps I Followed to Complete the Task

### 1️⃣ Initialize Terraform
This command downloads the AWS provider and prepares the working directory.

```bash
terraform init
```

---

### 2️⃣ Validate the Configuration
Ensures that the Terraform configuration has no syntax errors.

```bash
terraform validate
```

---

### 3️⃣ Review the Execution Plan
Shows what Terraform is going to create before actually creating it.

```bash
terraform plan
```

This step confirmed that:
- One VPC will be created
- CIDR block: `10.0.0.0/16`
- Tag name: `xfusion-vpc`

---

### 4️⃣ Apply the Configuration
Creates the VPC in AWS after confirmation.

```bash
terraform apply
```

Typed **yes** when prompted, and Terraform successfully created the VPC.

---

## ✅ Result

- VPC **xfusion-vpc** successfully created
- Resource added: **1**
- No errors or warnings
- Terraform state updated correctly

---

## 📜 Summary of Commands Used

| Command | Purpose |
|------|--------|
| `terraform init` | Initialize Terraform & download provider |
| `terraform validate` | Validate Terraform configuration |
| `terraform plan` | Preview infrastructure changes |
| `terraform apply` | Create the AWS VPC |

---

## 🎯 Key Takeaways

- Terraform makes AWS infrastructure predictable and repeatable
- Always run **plan** before **apply**
- Small labs like this build strong IaC fundamentals
- VPC creation is a core skill for cloud & DevOps engineers

---

🔁 **On to Day 95!**  
Consistent practice is the real superpower in DevOps 🚀
