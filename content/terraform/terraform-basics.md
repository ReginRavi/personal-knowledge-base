---
title: "Terraform Basics and Best Practices"
date: 2026-02-06T10:00:00+05:30
tags: ["terraform", "iac", "best-practices"]
draft: false
---

## What is Terraform?

Terraform is an Infrastructure as Code (IaC) tool that allows you to define and provision infrastructure using a declarative configuration language.

## Core Concepts

### Providers
Providers are plugins that interact with cloud platforms and other services:

```hcl
provider "aws" {
  region = "us-west-2"
}
```

### Resources
Resources are the most important element in Terraform:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "WebServer"
  }
}
```

### State Management

Terraform maintains state to track your infrastructure:

- **Local State**: Stored in `terraform.tfstate` file
- **Remote State**: Stored in backends like S3, Terraform Cloud

## Best Practices

1. **Use Remote State**: Store state remotely for team collaboration
2. **Version Control**: Keep `.tf` files in git, exclude `.tfstate`
3. **Use Modules**: Create reusable infrastructure components
4. **Plan Before Apply**: Always run `terraform plan` first
5. **Use Variables**: Make configurations flexible and reusable

```hcl
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}
```

## Workflow

```bash
terraform init    # Initialize working directory
terraform plan    # Preview changes
terraform apply   # Apply changes
terraform destroy # Destroy infrastructure
```
