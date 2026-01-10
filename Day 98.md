# Day 98: Creating a Private VPC with Subnet and EC2 using Terraform

## Introduction

In Day 98 of my **100 Days of KodeKloud Challenge**, I worked on setting
up a **private AWS infrastructure** using **Terraform**. The task was
focused on creating a Virtual Private Cloud (VPC), a subnet inside it,
and an EC2 instance that is accessible only within the VPC. This helps
in building a secure and isolated cloud environment.

## Understanding the Task in Simple Words

The main goal of this task was: - Create a **VPC** with a specific CIDR
range. - Create a **private subnet** inside that VPC. - Launch an **EC2
instance** inside that subnet. - Ensure that the EC2 instance can be
accessed **only from within the VPC**. - Use Terraform best practices
with variables and outputs.

In short, I was asked to build a **secure private network in AWS** where
resources are isolated from the public internet.

## Things You Should Know Before Doing This Task

Before attempting this task, it is helpful to understand the following:

1.  **Terraform Basics**
    -   What is Infrastructure as Code (IaC)
    -   Terraform files: `main.tf`, `variables.tf`, `outputs.tf`
2.  **AWS Networking Concepts**
    -   VPC (Virtual Private Cloud)
    -   Subnet (Public vs Private)
    -   CIDR blocks
    -   Security Groups
3.  **EC2 Basics**
    -   What an EC2 instance is
    -   AMI and instance types (like `t2.micro`)
4.  **Terraform Workflow**
    -   `terraform init`
    -   `terraform validate`
    -   `terraform plan`
    -   `terraform apply`

## Task Objective

The Nautilus DevOps team wanted to expand AWS infrastructure by: -
Creating a **private VPC** - Creating a **private subnet** (no public IP
assignment) - Launching a **t2.micro EC2 instance** - Allowing inbound
traffic **only from within the VPC** - Using variables for CIDR blocks -
Displaying resource names using outputs

## Implementation Overview

I structured the Terraform configuration into three files:

-   **main.tf** → Resource definitions (VPC, Subnet, Security Group,
    EC2)
-   **variables.tf** → Input variables for CIDR blocks
-   **outputs.tf** → Outputs to display resource names

### main.tf (Core Infrastructure)

-   Created a VPC with CIDR from variable `KKE_VPC_CIDR`
-   Created a subnet with CIDR from variable `KKE_SUBNET_CIDR`
-   Disabled public IP assignment using
    `map_public_ip_on_launch = false`
-   Created a security group that:
    -   Allows all traffic **only within the VPC**
    -   Allows all outbound traffic
-   Launched an EC2 instance (`t2.micro`) inside the private subnet

### variables.tf (Reusability)

-   Defined:
    -   `KKE_VPC_CIDR` → VPC CIDR block
    -   `KKE_SUBNET_CIDR` → Subnet CIDR block

### outputs.tf (Results)

-   Displayed:
    -   VPC name
    -   Subnet name
    -   EC2 instance name

## Steps I Followed to Complete the Task

1.  Initialized Terraform in the working directory:

    ``` bash
    terraform init
    ```

2.  Validated the configuration files:

    ``` bash
    terraform validate
    ```

3.  Reviewed the execution plan:

    ``` bash
    terraform plan
    ```

4.  Applied the configuration to create the resources:

    ``` bash
    terraform apply
    ```

After successful execution, Terraform created: - A private VPC - A
private subnet - A security group with restricted access - A private EC2
instance

## Summary of Commands Used

  Command              Purpose
  -------------------- ---------------------------------------------
  terraform init       Initialize Terraform and download providers
  terraform validate   Check configuration for syntax errors
  terraform plan       Preview the changes Terraform will apply
  terraform apply      Create and provision the AWS resources

## Final Thoughts

Day 98 helped me understand how to: - Build a **secure private AWS
network** - Use **Terraform variables** for flexibility - Control access
using **Security Groups** - Structure Terraform projects cleanly

This task was a great hands-on experience in real-world cloud
infrastructure automation. Looking forward to the next challenge in my
**100 Days of KodeKloud** journey!
