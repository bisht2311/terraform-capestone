
<div align="center">

# 🚀 Terraform Capstone Project

### Multi-Environment AWS Infrastructure with Terraform Workspaces & Custom Modules

**Infrastructure as Code | AWS | Terraform | Modular Architecture**

<br/>

![Terraform](https://img.shields.io/badge/Terraform-IaC-844FBA?style=for-the-badge&logo=terraform&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-Cloud-orange?style=for-the-badge&logo=amazonaws&logoColor=white)
![Infrastructure](https://img.shields.io/badge/Infrastructure-Automation-blue?style=for-the-badge)
![Modules](https://img.shields.io/badge/Terraform-Custom%20Modules-623CE4?style=for-the-badge)

<br/>

**A practical Infrastructure as Code project demonstrating reusable Terraform modules and multi-environment AWS infrastructure provisioning through a single Terraform codebase.**

</div>

---

## 📌 Table of Contents

- [📖 Project Overview](#-project-overview)
- [🎯 Project Objectives](#-project-objectives)
- [🏗️ Architecture](#️-architecture)
- [☁️ AWS Resources](#️-aws-resources)
- [🧠 Terraform Concepts](#-terraform-concepts)
- [📂 Project Structure](#-project-structure)
- [⚙️ Prerequisites](#️-prerequisites)
- [🔐 AWS Configuration](#-aws-configuration)
- [🛠️ Implementation](#️-implementation)
- [🚀 Deployment Guide](#-deployment-guide)
- [🌍 Multi-Environment Management](#-multi-environment-management)
- [🔍 Verification](#-verification)
- [🧹 Infrastructure Cleanup](#-infrastructure-cleanup)
- [💡 Key Learnings](#-key-learnings)
- [⚠️ Security Considerations](#️-security-considerations)
- [🔮 Future Enhancements](#-future-enhancements)
- [📚 References](#-references)

---

## 📖 Project Overview

This project demonstrates how to design, provision, and manage **AWS infrastructure using Terraform and Infrastructure as Code (IaC) principles.**

The implementation follows a modular architecture, where infrastructure components are organized into reusable Terraform modules.

Using **Terraform Workspaces**, the same codebase manages three independent environments:

<div align="center">

| 🧪 Development | 🔄 Staging | 🚀 Production |
|:---:|:---:|:---:|
| `dev` | `staging` | `prod` |
| Development & Testing | Pre-production Validation | Production Workloads |

</div>

### Why this architecture?

Managing separate Terraform configurations for different environments often results in duplicated code and inconsistent infrastructure.

This project addresses that challenge through:

- **Reusable Modules:** Organize infrastructure into independent components.
- **Terraform Workspaces:** Maintain separate state instances for different environments.
- **Input Variables:** Configure infrastructure without modifying resource definitions.
- **Local Values:** Centralize reusable expressions and environment-specific values.
- **Data Sources:** Retrieve existing AWS information dynamically.

### 🎯 Project Goal

Build a reusable, workspace-driven AWS infrastructure architecture that simplifies provisioning, improves maintainability, and demonstrates practical Terraform implementation.

---

## 🎯 Project Objectives

| Objective | Implementation |
|---|---|
| Infrastructure as Code | Terraform |
| Multi-environment management | Terraform Workspaces |
| Reusable infrastructure | Custom Modules |
| Configurable infrastructure | Input Variables |
| Centralized expressions | Local Values |
| Dynamic AWS information | Data Sources |
| Resource communication | Module Inputs & Outputs |
| Network access management | Security Groups |
| Infrastructure lifecycle | Terraform CLI |

---

## 🏗️ Architecture

### 1. AWS Infrastructure Architecture

The infrastructure provisions an AWS network environment containing a VPC, public subnet, Internet Gateway, routing configuration, Security Group, and EC2 instance.

```text
                    AWS CLOUD
                        |
                  +-------------+
                  |     VPC     |
                  | 10.0.0.0/16 |
                  +-------------+
                        |
                  +-------------+
                  |Public Subnet|
                  | 10.0.1.0/24 |
                  +-------------+
                        |
             +----------------------+
             |      Route Table     |
             |      0.0.0.0/0       |
             +----------------------+
                        |
                +---------------+
                | Internet      |
                | Gateway       |
                +---------------+

                        |
                +---------------+
                | Security Group|
                | SSH / HTTP    |
                +---------------+
                        |
                +---------------+
                | EC2 Instance  |
                | Amazon Linux 2|
                +---------------+
```

### 2. Terraform Module Architecture

The root module orchestrates three reusable child modules.

```text
                TERRAFORM ROOT MODULE
                         |
              +----------+----------+
              |          |          |
              ▼          ▼          ▼
           VPC        SECURITY     EC2
          MODULE      GROUP       MODULE
                      MODULE
              |          |          |
              ▼          ▼          ▼
           Network    Network     Compute
           Resources  Security    Resources
                      Rules
              |          |          |
              +----------+----------+
                         |
                    AWS PROVIDER
                         |
                    AWS CLOUD
```

### 3. Module Dependency Flow

Terraform automatically establishes dependencies through module output references.

```text
                 VPC MODULE
                      |
              +-------+-------+
              |               |
           VPC ID         Subnet ID
              |               |
              ▼               |
       SECURITY GROUP         |
          MODULE              |
              |               |
           SG ID              |
              |               |
              +-------+-------+
                      |
                      ▼
                  EC2 MODULE
                      |
                 EC2 INSTANCE
```

**Key concept:** The root module connects child modules by passing outputs from one module as inputs to another. This eliminates hardcoded resource IDs and establishes implicit dependencies.

---

## ☁️ AWS Resources

The project provisions the following AWS components:

| Resource | Description |
|---|---|
| **VPC** | Creates an isolated virtual network |
| **Public Subnet** | Hosts the EC2 instance |
| **Internet Gateway** | Enables internet connectivity |
| **Route Table** | Defines outbound traffic routing |
| **Route Association** | Associates the subnet with the route table |
| **Security Group** | Controls inbound and outbound traffic |
| **EC2 Instance** | Provides virtual compute capacity |

### Network Configuration

| Configuration | CIDR / Value |
|---|---|
| VPC | `10.0.0.0/16` |
| Public Subnet | `10.0.1.0/24` |
| Internet Route | `0.0.0.0/0` |
| Inbound Ports | `22, 80` |

*The values above are illustrative configuration examples.*

---

## 🧠 Terraform Concepts

This project provides hands-on implementation of the following Terraform concepts:

<details>
<summary><b>1. Terraform Workspaces</b></summary>

Workspaces allow multiple state instances to use the same Terraform configuration.

Implemented environments:

- Development
- Staging
- Production

Each workspace maintains its own state, allowing independent infrastructure management.

</details>

<details>
<summary><b>2. Custom Terraform Modules</b></summary>

Infrastructure is divided into reusable modules:

- VPC Module
- Security Group Module
- EC2 Instance Module

Each module contains its own resource definitions, input variables, and outputs.

**Benefits:**

- Code reusability
- Reduced duplication
- Separation of concerns
- Improved maintainability

</details>

<details>
<summary><b>3. Input Variables</b></summary>

Variables make Terraform configurations flexible and configurable.

Examples:

```hcl
variable "vpc_cidr" {
  type = string
}

variable "instance_type" {
  type = string
}
```

</details>

<details>
<summary><b>4. Local Values</b></summary>

Local values store reusable expressions.

```hcl
locals {
  environment = terraform.workspace

  name_prefix = "${var.project_name}-${local.environment}"
}
```

The current workspace determines the environment name.

</details>

<details>
<summary><b>5. Data Sources</b></summary>

Data sources retrieve information about existing AWS resources.

The project uses:

- `aws_ami` to retrieve a matching Amazon Linux 2 AMI.
- `aws_availability_zones` to retrieve available Availability Zones.

</details>

<details>
<summary><b>6. Dynamic Blocks</b></summary>

Dynamic blocks generate repeated nested configuration blocks.

Example:

```hcl
dynamic "ingress" {
  for_each = var.ingress_ports

  content {
    from_port   = ingress.value
    to_port     = ingress.value
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

This dynamically creates Security Group ingress rules based on the supplied port list.

</details>

<details>
<summary><b>7. Module Inputs and Outputs</b></summary>

Modules communicate through input variables and output values.

Example:

```hcl
vpc_id = module.vpc.vpc_id
```

This passes the VPC ID from the VPC module to the Security Group module.

</details>

---

## 📂 Project Structure

```text
terraform-capestone/
│
├── modules/
│   │
│   ├── vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   │
│   ├── security-group/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   │
│   └── ec2-instance/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
│
├── locals.tf
├── main.tf
├── outputs.tf
├── providers.tf
├── variables.tf
├── .gitignore
└── README.md
```

### Root Module

| File | Responsibility |
|---|---|
| `main.tf` | Data sources and module declarations |
| `providers.tf` | Terraform and AWS provider configuration |
| `variables.tf` | Root input variables |
| `locals.tf` | Reusable local expressions |
| `outputs.tf` | Infrastructure outputs |
| `.gitignore` | Excludes local and sensitive files |

### Child Module Structure

Every child module follows a consistent structure:

| File | Responsibility |
|---|---|
| `main.tf` | AWS resource definitions |
| `variables.tf` | Module input parameters |
| `outputs.tf` | Exposes resource attributes |

---

## ⚙️ Prerequisites

Before starting, ensure the following tools are installed:

| Tool | Purpose | Documentation |
|---|---|---|
| Terraform | Infrastructure provisioning | [Install Terraform](https://developer.hashicorp.com/terraform/install) |
| AWS CLI | AWS authentication and management | [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) |
| AWS Account | Cloud infrastructure | [AWS Console](https://aws.amazon.com/console/) |

Verify installations:

```bash
terraform version
aws --version
```

---

## 🔐 AWS Configuration

Configure AWS credentials using the AWS CLI.

```bash
aws configure
```

Provide your credentials:

```text
AWS Access Key ID: YOUR_ACCESS_KEY
AWS Secret Access Key: YOUR_SECRET_KEY
Default region name: ap-south-1
Default output format: json
```

Verify authentication:

```bash
aws sts get-caller-identity
```

This command returns the AWS account and IAM identity associated with the configured credentials.

> **Security:** Never commit AWS credentials or Terraform state files to GitHub. Prefer IAM roles and temporary credentials wherever possible.

---

## 🛠️ Implementation

### Step 1: Configure the AWS Provider

File: `providers.tf`

```hcl
terraform {
  required_version = ">= 1.10.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "ap-south-1"
}
```

**Implementation details:**

- Specifies Terraform version constraints.
- Declares the AWS provider.
- Restricts the provider to compatible 5.x releases.
- Configures the AWS deployment region.

### Step 2: Define Input Variables

File: `variables.tf`

Input variables make the infrastructure configurable without modifying resource definitions.

Example variables:

```hcl
variable "project_name" {
  type    = string
  default = "terraweek-capestone"
}

variable "vpc_cidr" {
  type = string
}

variable "subnet_cidr" {
  type = string
}

variable "instance_type" {
  type = string
}

variable "ingress_ports" {
  type    = list(number)
  default = [22, 80]
}
```

### Step 3: Configure Local Values

File: `locals.tf`

```hcl
locals {
  environment = terraform.workspace

  name_prefix = "${var.project_name}-${local.environment}"

  common_tags = {
    Project     = var.project_name
    Environment = local.environment
    ManagedBy   = "Terraform"
    Workspace   = terraform.workspace
  }
}
```

**Implementation details:**

- `terraform.workspace` identifies the active workspace.
- `local.environment` stores the environment name.
- `local.name_prefix` generates environment-specific naming.
- `local.common_tags` defines reusable resource metadata.

### Step 4: Implement the VPC Module

Directory: `modules/vpc/`

The VPC module provisions the networking layer:

- VPC
- Public Subnet
- Internet Gateway
- Route Table
- Route Table Association

Example route configuration:

```hcl
route {
  cidr_block = "0.0.0.0/0"
  gateway_id = aws_internet_gateway.igw.id
}
```

Module outputs:

```hcl
output "vpc_id" {
  value = aws_vpc.vpc.id
}

output "subnet_id" {
  value = aws_subnet.public_subnet.id
}
```

These outputs are consumed by other modules.

### Step 5: Implement the Security Group Module

Directory: `modules/security-group/`

The Security Group module manages network access rules.

The module accepts the VPC ID and configurable ingress ports.

Example dynamic ingress block:

```hcl
dynamic "ingress" {
  for_each = var.ingress_ports

  content {
    from_port   = ingress.value
    to_port     = ingress.value
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

For a port list of `[22, 80]`, Terraform generates two ingress rules.

Module output:

```hcl
output "sg_id" {
  value = aws_security_group.sg.id
}
```

### Step 6: Implement the EC2 Module

Directory: `modules/ec2-instance/`

The EC2 module provisions compute resources using configurable inputs.

```hcl
resource "aws_instance" "ec2" {
  ami                    = var.ami_id
  instance_type          = var.instance_type
  subnet_id              = var.subnet_id
  vpc_security_group_ids = var.security_group_ids

  associate_public_ip_address = true
}
```

Module outputs:

```hcl
output "instance_id" {
  value = aws_instance.ec2.id
}

output "public_ip" {
  value = aws_instance.ec2.public_ip
}
```

### Step 7: Configure AWS Data Sources

File: `main.tf`

The Amazon Linux 2 AMI is retrieved dynamically.

```hcl
data "aws_ami" "amazon_linux_2_gp3" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-*"]
  }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }

  filter {
    name   = "architecture"
    values = ["x86_64"]
  }

  filter {
    name   = "root-device-type"
    values = ["ebs"]
  }
}
```

Availability Zone lookup:

```hcl
data "aws_availability_zones" "available" {
  state = "available"
}
```

These data sources retrieve AWS information without manually specifying resource identifiers.

### Step 8: Connect the Modules

File: `main.tf`

The root module orchestrates the child modules.

**VPC Module**

```hcl
module "vpc" {
  source             = "./modules/vpc"
  cidr               = var.vpc_cidr
  public_subnet_cidr = var.subnet_cidr
  environment        = local.environment
  project_name       = var.project_name
}
```

**Security Group Module**

```hcl
module "security_group" {
  source        = "./modules/security-group"
  vpc_id        = module.vpc.vpc_id
  ingress_ports = var.ingress_ports
  environment   = local.environment
  project_name  = var.project_name
}
```

**EC2 Module**

```hcl
module "ec2" {
  source = "./modules/ec2-instance"

  ami_id             = data.aws_ami.amazon_linux_2_gp3.id
  instance_type      = var.instance_type
  subnet_id          = module.vpc.subnet_id
  security_group_ids = [module.security_group.sg_id]
  environment        = local.environment
  project_name       = var.project_name
}
```

**Module communication:**

| Source | Output | Consumer |
|---|---|---|
| VPC Module | `vpc_id` | Security Group Module |
| VPC Module | `subnet_id` | EC2 Module |
| Security Group Module | `sg_id` | EC2 Module |
| AMI Data Source | `id` | EC2 Module |

This creates a modular infrastructure architecture without hardcoding AWS resource IDs.

### Step 9: Define Root Outputs

File: `outputs.tf`

```hcl
output "vpc_id" {
  value = module.vpc.vpc_id
}

output "subnet_id" {
  value = module.vpc.subnet_id
}

output "security_group_id" {
  value = module.security_group.sg_id
}

output "instance_id" {
  value = module.ec2.instance_id
}

output "public_ip" {
  value = module.ec2.public_ip
}
```

Terraform displays these values after successful deployment.

---

## 🚀 Deployment Guide

Follow the steps below to provision your AWS infrastructure.

### 1. Clone the Repository

```bash
git clone https://github.com/bisht2311/terraform-capestone.git

cd terraform-capestone
```

### 2. Initialize Terraform

```bash
terraform init
```

Initializes the working directory and downloads required providers and modules.

### 3. Format and Validate

```bash
terraform fmt -recursive

terraform validate
```

### 4. Create Workspaces

```bash
terraform workspace new dev
terraform workspace new staging
terraform workspace new prod
```

Verify:

```bash
terraform workspace list
```

### 5. Configure Environment Variables

Create `dev.tfvars`:

```hcl
vpc_cidr      = "10.0.0.0/16"
subnet_cidr   = "10.0.1.0/24"
instance_type = "t2.micro"

ingress_ports = [22, 80]
```

Create `staging.tfvars`:

```hcl
vpc_cidr      = "10.1.0.0/16"
subnet_cidr   = "10.1.1.0/24"
instance_type = "t2.micro"

ingress_ports = [22, 80]
```

Create `prod.tfvars`:

```hcl
vpc_cidr      = "10.2.0.0/16"
subnet_cidr   = "10.2.1.0/24"
instance_type = "t3.micro"

ingress_ports = [22, 80]
```

*These are example configurations. Ensure the selected instance types are available in your AWS region.*

### 6. Generate Execution Plan

For Development:

```bash
terraform workspace select dev

terraform plan -var-file="dev.tfvars"
```

### 7. Apply Infrastructure

```bash
terraform apply -var-file="dev.tfvars"
```

Review the execution plan and confirm by entering:

```text
yes
```

Terraform will provision the AWS infrastructure.

### 8. View Outputs

```bash
terraform output
```

Expected output structure:

```text
vpc_id            = "vpc-xxxxxxxx"
subnet_id         = "subnet-xxxxxxxx"
security_group_id = "sg-xxxxxxxx"
instance_id       = "i-xxxxxxxx"
public_ip         = "x.x.x.x"
```

---

## 🌍 Multi-Environment Management

The same Terraform codebase can manage multiple environments using workspace selection and environment-specific variable files.

| Environment | Workspace | Variable File |
|---|---|---|
| Development | `dev` | `dev.tfvars` |
| Staging | `staging` | `staging.tfvars` |
| Production | `prod` | `prod.tfvars` |

### Development

```bash
terraform workspace select dev
terraform plan -var-file="dev.tfvars"
terraform apply -var-file="dev.tfvars"
```

### Staging

```bash
terraform workspace select staging
terraform plan -var-file="staging.tfvars"
terraform apply -var-file="staging.tfvars"
```

### Production

```bash
terraform workspace select prod
terraform plan -var-file="prod.tfvars"
terraform apply -var-file="prod.tfvars"
```

### Workspace Isolation

Each workspace maintains its own Terraform state.

```text
             SINGLE TERRAFORM CODEBASE
                        |
                WORKSPACE SELECTION
                        |
           +------------+------------+
           |            |            |
           ▼            ▼            ▼
          DEV         STAGING       PROD
           |            |            |
           ▼            ▼            ▼
        STATE         STATE         STATE
           |            |            |
           ▼            ▼            ▼
        AWS INFRA   AWS INFRA    AWS INFRA
```

> **Note:** Workspaces provide state separation, not complete security or administrative isolation. For production-grade isolation, separate AWS accounts and appropriately isolated backends are recommended.

---

## 🔍 Verification

After deployment, verify the infrastructure.

**Check current workspace**

```bash
terraform workspace show
```

**List Terraform-managed resources**

```bash
terraform state list
```

**Verify VPC**

```bash
aws ec2 describe-vpcs \
  --filters "Name=tag:Project,Values=terraweek-capestone"
```

**Verify EC2 instance**

```bash
aws ec2 describe-instances \
  --filters "Name=tag:Project,Values=terraweek-capestone" \
  --query "Reservations[].Instances[].[InstanceId,State.Name,PublicIpAddress]"
```

**Verify Security Groups**

```bash
aws ec2 describe-security-groups \
  --filters "Name=tag:Project,Values=terraweek-capestone"
```

You can also verify the provisioned resources through the AWS Management Console.

---

## 🧹 Infrastructure Cleanup

To avoid unnecessary AWS charges, destroy unused infrastructure.

Always select the correct workspace before destroying resources.

**Development**

```bash
terraform workspace select dev
terraform destroy -var-file="dev.tfvars"
```

**Staging**

```bash
terraform workspace select staging
terraform destroy -var-file="staging.tfvars"
```

**Production**

```bash
terraform workspace select prod
terraform destroy -var-file="prod.tfvars"
```

Confirm the destruction after reviewing the proposed changes.

**Delete an unused workspace:**

```bash
terraform workspace select default
terraform workspace delete dev
```

Only delete a workspace after its infrastructure has been destroyed and its state is no longer needed.

---

## 💡 Key Learnings

Through this project, I gained practical experience in:

- Designing reusable Terraform module architectures.
- Implementing multi-environment infrastructure using Workspaces.
- Managing AWS networking and compute resources through IaC.
- Passing resource attributes between modules using Inputs and Outputs.
- Implementing dynamic Security Group rules.
- Using Data Sources for dynamic AWS resource discovery.
- Applying Local Values for environment-based configuration.
- Managing resource dependencies through Terraform references.
- Executing the complete Terraform infrastructure lifecycle.

### Challenges & Solutions

| Challenge | Solution |
|---|---|
| Infrastructure duplication | Introduced reusable custom modules |
| Multi-environment management | Implemented Terraform Workspaces |
| Module communication | Used input variables and outputs |
| Dynamic security rules | Implemented Dynamic Blocks |
| Resource identification | Applied environment-specific tags |
| AWS resource discovery | Implemented Data Sources |
| Resource creation order | Used implicit dependencies |

---

## ⚠️ Security Considerations

This is a hands-on learning project. The following improvements are recommended before production adoption.

1. **Restrict SSH access:** Avoid exposing port 22 to `0.0.0.0/0`.
2. **Protect Terraform State:** Use an encrypted remote backend with access controls.
3. **Use IAM Roles:** Prefer temporary credentials over hardcoded AWS keys.
4. **Separate Environments:** Consider separate AWS accounts for stronger production isolation.
5. **Control AWS Costs:** Destroy unused infrastructure after testing.

---

## 🔮 Future Enhancements

Potential improvements to extend this project:

- [ ] S3 Remote Backend for centralized state management.
- [ ] Separate AWS accounts for Dev, Staging, and Production.
- [ ] Private Subnets and NAT Gateway.
- [ ] Application Load Balancer.
- [ ] Auto Scaling Groups.
- [ ] IAM Roles and least-privilege policies.
- [ ] GitHub Actions for Terraform CI/CD.
- [ ] Security scanning using Checkov or tfsec.
- [ ] Automated Terraform infrastructure testing.

---

## 📚 References

Official documentation used during implementation:

| Resource | Documentation |
|---|---|
| Terraform | [Official Documentation](https://developer.hashicorp.com/terraform/docs) |
| Terraform Workspaces | [Workspaces](https://developer.hashicorp.com/terraform/language/state/workspaces) |
| Terraform Modules | [Modules](https://developer.hashicorp.com/terraform/language/modules) |
| Terraform Variables | [Input Variables](https://developer.hashicorp.com/terraform/language/values/variables) |
| Terraform Locals | [Local Values](https://developer.hashicorp.com/terraform/language/values/locals) |
| Terraform Data Sources | [Data Sources](https://developer.hashicorp.com/terraform/language/data-sources) |
| AWS Provider | [AWS Registry](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) |

---

## 👨‍💻 Author

### Harshit Bisht

**Cloud & DevOps Enthusiast**

[![GitHub](https://img.shields.io/badge/GitHub-bisht2311-181717?style=for-the-badge&logo=github)](https://github.com/bisht2311)
[![Repository](https://img.shields.io/badge/Project-terraform--capestone-623CE4?style=for-the-badge&logo=terraform)](https://github.com/bisht2311/terraform-capestone)

**Building reusable infrastructure. Automating cloud operations. Learning by implementing.**

⭐ If you find this project useful, consider giving the repository a star!

---
