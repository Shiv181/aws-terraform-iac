# aws-terraform-iac
# Automated Infrastructure with Terraform

![Project Overview](images/terraform-card.png)

## Overview

This project codifies a complete AWS environment using **Terraform** for one-click provisioning. Resources include:

* **VPC** with public/private subnets and NAT gateways
* **EC2** instances for application servers
* **Amazon RDS** database cluster
* **AWS Lambda** functions
* **CloudFront** distribution for static assets
* **Route 53** DNS records for custom domains

By leveraging Infrastructure as Code (IaC), deployments become repeatable, auditable, and easily version-controlled.

## Architecture Diagram

```text
Terraform Configuration → AWS Provisioning:
  └─ VPC (Public & Private Subnets)
  └─ Internet & NAT Gateways
  └─ Security Groups & IAM Roles
  └─ EC2 Auto Scaling Group
  └─ RDS (Multi-AZ)
  └─ Lambda Functions
  └─ CloudFront + S3 Static Assets
  └─ Route 53 DNS Records
```

## Prerequisites

* **Terraform** v1.3+ installed locally
* **AWS CLI** configured with appropriate credentials and region
* Git repository cloned: `git clone git@github.com:Shiv181/terraform-automation.git`

## Repository Structure

```bash
├── modules/              # Reusable Terraform modules (VPC, EC2, RDS, etc.)
│   ├── vpc/
│   ├── ec2/
│   └── rds/
├── env/                  # Environment-specific variable overrides
│   ├── prod.tfvars
│   └── dev.tfvars
├── main.tf               # Root module linking sub-modules
├── variables.tf          # Global variable definitions
├── outputs.tf            # Exported resource attributes
├── provider.tf           # Provider and backend configuration
├── scripts/              # Helper scripts (e.g., bootstrap, migrations)
└── images/               # Screenshots used in this README
    ├── terraform-init.png
    ├── terraform-plan.png
    ├── terraform-apply.png
    └── architecture-diagram.png
```

## Setup & Deployment

### 1. Initialize Terraform

```bash
cd terraform-automation
tfenv install && terraform init
```

![Terraform Init](images/terraform-init.png)

### 2. Preview Changes

Generate an execution plan to verify resource changes before applying:

```bash
terraform plan -var-file=env/dev.tfvars
```

![Terraform Plan](images/terraform-plan.png)

### 3. Apply Configuration

Provision all resources in AWS:

```bash
terraform apply -var-file=env/dev.tfvars
```

![Terraform Apply](images/terraform-apply.png)

### 4. Verify Deployment

* Check the **VPC** and **subnet** resources in the AWS console.
* Ensure **EC2** instances are running and reachable via their security groups.
* Connect to **RDS** endpoint using provided credentials.
* Trigger **Lambda** functions manually or via test events.
* Access the **CloudFront** URL and confirm custom domain routing via **Route 53**.

## State Management

* Remote state is configured in an S3 bucket with DynamoDB locking (see `provider.tf`).
* For team workflows, ensure `terraform init` includes backend configuration.

## Cleanup

To tear down all provisioned infrastructure:

```bash
terraform destroy -var-file=env/dev.tfvars
```

## Contributing

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature`.
3. Commit your changes and open a pull request.
