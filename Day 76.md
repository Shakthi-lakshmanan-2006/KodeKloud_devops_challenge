# Day 76: Jenkins Project Security

## Introduction
In this task, the objective is to configure **project-based security in Jenkins**. The challenge involves managing user permissions at the job level using the **Matrix Authorization Strategy** plugin and applying custom permissions to two specific users for an existing Jenkins job named **Packages**.

## Concepts to Know Before Starting

### 🔐 Jenkins Authorization Strategies
- Global Authorization controls permissions for the entire Jenkins instance.
- Project-Based Matrix Authorization Strategy allows permission control per job.

### 👤 User Permissions in Jenkins
- Read, Build, Configure, Cancel, Update, Tag

### 🔌 Plugins Required
- Matrix Authorization Strategy Plugin

## Steps Followed in the Task

1. Login to Jenkins (admin / Adm!n321).
2. Install Matrix Authorization Strategy plugin.
3. Enable Project-based Matrix Authorization Strategy.
4. Grant read access to users: sam, rohan.
5. Configure Packages job:
   - Enable project-based security
   - Inherit permissions from parent ACL
   - Assign permissions:
     - sam: read, build, configure
     - rohan: build, cancel, configure, read, update, tag

## Summary of Commands Used
This task is fully GUI-based; no terminal commands used.
