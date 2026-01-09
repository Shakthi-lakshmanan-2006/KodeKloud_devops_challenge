# Day 97 – Creating an IAM Policy Using Terraform (KodeKloud Engineer)

## 📌 Challenge Title
**Create an IAM Policy for EC2 Read-Only Access Using Terraform**

---

## 🔰 Introduction

As part of **Day 97 of my 100 Days KodeKloud Challenge**, I worked on configuring AWS Identity and Access Management (IAM) using **Terraform**.  
IAM is one of the most critical AWS services because it controls **who can access what** in your AWS account.

In this task, the goal was to create an **IAM policy** that provides **read-only access to the EC2 console**, using Terraform as the Infrastructure as Code (IaC) tool.

---

## 🧩 Problem Statement (Simplified)

The Nautilus DevOps team wants to:

- Create an **IAM policy** named `iampolicy_kareem`
- Use **Terraform**
- Deploy it in the **us-east-1** region
- Allow **read-only access** to:
  - EC2 instances
  - AMIs
  - Snapshots
- The Terraform working directory is:
  ```
  /home/bob/terraform
  ```
- The policy must be created using a **single `main.tf` file**

---
![Screenshot](./assets/Screenshot%202026-01-03%20202222.png)

---

## 🧠 Things to Know Before Starting

Before working on this task, it’s helpful to understand:

- **IAM Policies**: JSON documents that define permissions
- **Terraform Basics**:
  - `terraform init`
  - `terraform validate`
  - `terraform plan`
  - `terraform apply`
- **AWS Provider** in Terraform
- **jsonencode()** function to convert Terraform syntax into valid JSON
- IAM policy naming rules (no trailing spaces, only allowed characters)

---

## ⚙️ Approach Overview

To solve this task, I:

1. Created a `main.tf` file in the given directory
2. Defined an `aws_iam_policy` resource
3. Used `jsonencode()` to define the IAM policy document
4. Validated the Terraform configuration
5. Planned and applied the changes to AWS

---

## 📝 Terraform Configuration (main.tf)

```hcl
resource "aws_iam_policy" "policy" {
  name        = "iampolicy_kareem"
  path        = "/"
  description = "EC2ReadOnlyAccess Policy"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "ec2:Describe*",
          "ec2:GetSecurityGroupsForVpc"
        ]
        Resource = "*"
      },
      {
        Effect   = "Allow"
        Action   = "elasticloadbalancing:Describe*"
        Resource = "*"
      },
      {
        Effect = "Allow"
        Action = [
          "cloudwatch:ListMetrics",
          "cloudwatch:GetMetricStatistics",
          "cloudwatch:Describe*"
        ]
        Resource = "*"
      },
      {
        Effect   = "Allow"
        Action   = "autoscaling:Describe*"
        Resource = "*"
      }
    ]
  })
}
```

---
![Screenshot](./assets/Screenshot%202026-01-03%20203030.png)

---

## 🚀 Steps Executed in the Terminal

```bash
terraform init
terraform validate
terraform plan
terraform apply
```

### 🔍 Key Observation
- Initially, `terraform validate` failed due to a **trailing space** in the policy name.
- Removing the extra space fixed the issue.

---

## 📊 Output Result

- IAM Policy **iampolicy_kareem** was successfully created
- Terraform confirmed:
  ```
  Resources: 1 added, 0 changed, 0 destroyed
  ```

---

## ✅ Summary of Commands Used

| Command | Purpose |
|------|--------|
| `terraform init` | Initialize Terraform & providers |
| `terraform validate` | Check syntax and configuration |
| `terraform plan` | Preview infrastructure changes |
| `terraform apply` | Create the IAM policy |

---

## 🎯 Conclusion

This task was a great hands-on experience in:

1. Writing IAM policies using Terraform
2. Understanding AWS permission boundaries
3. Debugging Terraform validation errors
4. Practicing Infrastructure as Code best practices

✅ **Day 97 successfully completed!**  
On to the next challenge 🚀

---
![Screenshot](./assets/Screenshot%202026-01-03%20203552.png)

---
