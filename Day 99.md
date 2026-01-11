# Day 99 -- Fixing Terraform Duplicate Provider Error

## 🚨 Issue Encountered

While running the following command:

``` bash
terraform init
```

I encountered this error:

    Error: Duplicate provider configuration

    A default (non-aliased) provider configuration for "aws" was already given.
    If multiple configurations are required, set the "alias" argument for alternative configurations.

This prevented Terraform from initializing properly.

------------------------------------------------------------------------

## 🔍 Root Cause

Terraform does **not allow more than one default provider block** for
the same provider unless aliases are used.

In my project, the AWS provider was defined in **two places**:

-   `main.tf`
-   `provider.tf`

Terraform detected **two identical `provider "aws"` blocks** and failed
during initialization.

------------------------------------------------------------------------

## ❌ What Was Wrong

The project structure looked like this:

    main.tf       → provider "aws" {...}
    provider.tf   → provider "aws" {...}

This caused Terraform to throw a **duplicate provider configuration**
error.

------------------------------------------------------------------------

## ✅ Correct Fix

Since the task instructions clearly stated:

> **"Create the main.tf file (do not create a separate .tf file)"**

The correct solution was:

👉 **Remove the provider block from `provider.tf` and keep it only in
`main.tf`**

------------------------------------------------------------------------

## 🛠 Updated Configuration

### ✅ `main.tf` (Keep Provider Here)

``` hcl
provider "aws" {
  region = "us-east-1"
}
```

### ❌ `provider.tf` (Delete or Remove Provider Block)

Either: - Delete the entire file\
**OR** - Remove this block from it:

``` hcl
provider "aws" {
  region = "us-east-1"
}
```

------------------------------------------------------------------------

## 🔁 Re-run Terraform

After fixing the duplicate provider issue:

``` bash
terraform init
terraform plan
terraform apply
```

Terraform initialized successfully without errors.

------------------------------------------------------------------------

## 🧠 Key Takeaways

✔ Terraform allows **only one default provider** per provider type\
✔ Multiple providers require **aliases** (not needed in this task)\
✔ Follow lab instructions carefully\
✔ KodeKloud labs may already include `provider.tf` by default

------------------------------------------------------------------------

## 🎯 Summary

On **Day 99 of my 100 Days of KodeKloud Challenge**, I resolved a common
Terraform error caused by **duplicate AWS provider definitions**. By
keeping the provider configuration only in `main.tf` and removing it
from `provider.tf`, Terraform initialized successfully and the
infrastructure was provisioned correctly.

🚀 **Lesson learned: One provider block per provider --- unless aliases
are required!**
