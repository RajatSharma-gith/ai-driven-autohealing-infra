# AI-Driven Auto-Healing Infrastructure

This repository contains Infrastructure as Code (IaC) written in Terraform to deploy a self-healing environment on AWS, enhanced with machine learning capabilities via Amazon SageMaker. 

The project provisions an EC2 instance protected by a CloudWatch Alarm that automatically triggers instance recovery if the system fails. Furthermore, it sets up an environment for deploying a predictive AI model (using XGBoost) to anticipate failures and proactively manage infrastructure health.

## Key Features

- **Automated Instance Recovery**: Utilizes Amazon CloudWatch Alarms to monitor the `StatusCheckFailed_System` metric (and CPU utilization) to automatically recover unresponsive EC2 instances.
- **AI/ML Integration**: Provisions an Amazon SageMaker model endpoint using an XGBoost container for predictive analytics on infrastructure health.
- **Artifact Management**: Automatically creates an Amazon S3 bucket to store the SageMaker `model.tar.gz` and provisions an Amazon ECR repository to manage custom container images.
- **IAM Security**: Configures the necessary least-privilege IAM roles and instance profiles for CloudWatch, EC2, and SageMaker.
- **OSM Demo Application**: Includes a directory (`osm`) containing the frontend, backend, and infrastructure configuration for a sample application deployed alongside the auto-healing architecture.

## Project Structure

```text
.
├── ai-auto-healing/         # Terraform configurations for the Auto-Healing & AI infrastructure
│   ├── main.tf              # Main resources: EC2, CloudWatch, SageMaker, S3, ECR, IAM
│   ├── variables.tf         # Input variables (Region, AMI, Subnet, etc.)
│   ├── output.tf            # Output values (IPs, Endpoints, Bucket Name)
│   └── provider.tf          # AWS Provider configuration
└── osm/                     # Demo application repository
    ├── front-end/
    ├── newsfeed/
    ├── quotes/
    ├── infra/
    ├── APP_BUILD.md
    └── MANUAL_SETUP.md
```

## Prerequisites

- [Terraform](https://www.terraform.io/downloads.html) installed (v1.0.0+)
- AWS CLI configured with active credentials (`aws configure`)
- Appropriate AWS IAM permissions to create EC2, IAM, S3, ECR, CloudWatch, and SageMaker resources.

## Deployment Instructions

1. **Clone the repository:**
   ```bash
   git clone <your-repo-url>
   cd AI-Driven-Auto-Healing-Infrastructure
   ```

2. **Navigate to the infrastructure directory:**
   ```bash
   cd ai-auto-healing
   ```

3. **Initialize Terraform:**
   ```bash
   terraform init
   ```

4. **Review the deployment plan:**
   ```bash
   terraform plan -var="subnet_id=<your-subnet-id>"
   ```
   *(Note: You can also define your subnet ID in a `terraform.tfvars` file to avoid passing it via the command line.)*

5. **Apply the configuration:**
   ```bash
   terraform apply
   ```
   Confirm with `yes` when prompted.

6. **View the Outputs:**
   Upon successful completion, Terraform will output the public IP of your EC2 instance, the SageMaker endpoint name, the S3 bucket name, and the ECR repository URL.

## Clean Up

To avoid incurring future charges on AWS, remember to tear down the infrastructure when it is no longer needed:

```bash
cd ai-auto-healing
terraform destroy
```

## License

Please refer to the `LICENSE` file in the subdirectories (e.g., `osm/LICENSE`) for information on the licensing terms for the provided demo application.
