# team-alpha-infra

Infrastructure-as-code for Team Alpha's AWS cloud deployment using Terraform.

## Architecture

This project deploys a three-tier AWS infrastructure:

- **Web Tier**: Public subnet with EC2 instances behind security groups, accessible via SSH (port 22)
- **App Tier**: Private subnet with EC2 instances, accessible only from the web tier
- **Data Tier**: Isolated subnets with RDS database subnet group

Supporting infrastructure includes:
- VPC with configurable CIDR blocks and multiple availability zones
- NAT Gateway for app tier egress (optional, incurs costs)
- Security groups for each tier
- S3 buckets for application assets and static website hosting
- IAM roles and instance profiles for EC2 access to S3

## Prerequisites

- [Terraform](https://www.terraform.io/downloads.html) >= 1.0
- AWS CLI configured with appropriate credentials
- Access to the AWS account and S3 bucket for Terraform state

## Quick Start

1. **Initialize Terraform**:
   ```bash
   terraform init
   ```

2. **Review the plan**:
   ```bash
   terraform plan
   ```

3. **Apply the configuration**:
   ```bash
   terraform apply
   ```

## Configuration

### Variables

Key variables can be customized via `terraform.tfvars` or command line:

| Variable | Default | Description |
|----------|---------|-------------|
| `aws_region` | `eu-west-2` | AWS region |
| `project_name` | `mupando` | Project name prefix |
| `environment` | `dev` | Environment identifier |
| `vpc_cidr` | `10.0.0.0/16` | VPC CIDR block |
| `public_subnet_cidrs` | `["10.0.1.0/24", "10.0.2.0/24"]` | Public subnet CIDRs |
| `app_subnet_cidrs` | `["10.0.11.0/24", "10.0.12.0/24"]` | App tier subnet CIDRs |
| `db_subnet_cidrs` | `["10.0.21.0/24", "10.0.22.0/24"]` | Database subnet CIDRs |
| `enable_nat_gateway` | `false` | Create NAT Gateway (incurs hourly charges) |
| `instance_type` | `t2.micro` | EC2 instance type |
| `ssh_cidr_blocks` | `["0.0.0.0/0"]` | CIDR ranges allowed SSH access |

### Backend State

Terraform state is stored in S3 (see `backend.tf`). Ensure the S3 bucket exists and your AWS credentials have access.

## File Structure

```
.
├── main.tf                 # Primary resource definitions (VPC, modules)
├── providers.tf            # AWS provider configuration
├── backend.tf              # S3 backend for Terraform state
├── variables.tf            # Input variable definitions
├── outputs.tf              # Output values
├── sg.tf                   # Security group definitions
├── terraform.tfvars.example # Example variable values
└── user_data/              # User data scripts for EC2 instances
    ├── web.sh.tftpl        # Web tier initialization
    └── app.sh.tftpl        # App tier initialization
```

## Modules

This configuration uses external Terraform modules from `pro-digital-uk`:

- `aws-vpc-module` — VPC and subnet management
- `aws-ec2-module` — EC2 instance provisioning
- `aws-s3-module` — S3 bucket creation

## Deployment

To deploy to a specific environment, create a `terraform.tfvars` file:

```hcl
aws_region     = "eu-west-2"
project_name   = "myproject"
environment    = "prod"
instance_type  = "t3.small"
```

Then run:
```bash
terraform init
terraform plan -out=tfplan
terraform apply tfplan
```

## Outputs

After applying, retrieve outputs with:

```bash
terraform output vpc_id
terraform output public_subnet_ids
terraform output app_subnet_ids
```

## Cleanup

To destroy all infrastructure:

```bash
terraform destroy
```

## Notes

- **NAT Gateway Cost**: Enabling `enable_nat_gateway` incurs hourly charges plus data processing costs. Use only for environments that need app tier egress.
- **State Management**: The `.terraform.lock.hcl` file is committed to version control to ensure reproducible deployments.
- **Sensitive Data**: Never commit `*.tfvars` files containing secrets. Use `.tfvars.example` as a template.

## Support

For issues or questions, refer to the Terraform AWS provider documentation or contact the Team Alpha infrastructure team.
