---
layout: default
title: Setup
parent: Running Vulture on AWS cloud with Nextflow
nav_order: 1
---

To get started with Vulture, you will need a AWS account and setup the environment in your account. Vulture is composed of a stack of AWS services as below:


# Setup
This chapter guides you to configure the following AWS services which you will use in Vulture. 
- AWS IAM
- AWS Batch
- AWS S3

## Create an IAM role for your Workspace

1. Head over to the [IAM console](https://console.aws.amazon.com/iam/home), find and click "Create role" button **(2)** under the Roles **(1)** section.

![Image](../../src/img/IAM_Role/Vulture 2-1 Create Role.png)

2. Select the "AWS service" **(3)** and choose the "Batch" use case **(4)** hit Next button **(5)** at the bottom.

![Image](../../src/img/IAM_Role/Vulture 2-2 create role - select batch.png)
![Image](../../src/img/IAM_Role/Vulture 2-3 select batch service role.png)

3. Select the following policies and click "attach policies" to add them **(6)**.

- AdministratorAccess
- AmazonEC2FullAccess
- AmazonEC2ContainerRegistryFullAccess
- AmazonS3FullAccess
- AWSBatchServiceRole
- AdministratorAccess
- AWSBatchFullAccess
- AmazonEC2ContainerServiceforEC2Role
- AmazonEC2ContainerServiceRole

![Image](../../src/img/IAM_Role/vulture 2-4 attach other roles.png)

4. Name the role as "vulture-iam-role" and click "Create role".

![Image](../../src/img/IAM_Role/vulture 2-5 create role iam role complete.png)


Therefore, a role named "aws-workshop-admin" is ready for use.
