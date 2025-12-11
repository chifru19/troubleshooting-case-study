# 🚀 Serverless Infrastructure Refactoring Project (Terraform & LocalStack)

## Project Overview

This project demonstrates the **refactoring and deployment of a modular Serverless data processing pipeline** using **Terraform** and the local cloud environment, **LocalStack**.

The goal was to transform a monolithic Terraform configuration into a robust, reusable module, while simultaneously solving complex environmental and authentication challenges inherent to using cloud infrastructure as code (IaC) with local testing tools.

### Key Skills Demonstrated

- **Infrastructure as Code (IaC):** Modular design and deployment using Terraform.
- **DevOps/Testing:** Utilizing **LocalStack** to emulate AWS services (S3, Lambda, IAM) for cost-effective, local testing.
- **Cloud Architecture:** Provisioning a serverless S3 $\rightarrow$ Lambda event-driven pipeline.
- **Troubleshooting:** Advanced resolution of authentication (`InvalidAccessKeyId`) and networking (`Connection Refused`) errors in a Dockerized environment.

---

## ☁️ Architecture & Components

The deployed infrastructure creates a basic, event-driven serverless pipeline:

| **Component** | **Resource** | **Purpose** |
| --- | --- | --- |
| **Data Ingestion** | `aws_s3_bucket` | Stores incoming data (named `localstack-modular-trigger-bucket-2`). |
| **Compute** | `aws_lambda_function` | Runs the processing logic (e.g., Python code). |
| **Security** | `aws_iam_role` | Grants the Lambda function necessary permissions to execute and read logs. |
| **Trigger** | `aws_s3_bucket_notification` | Configures the S3 bucket to invoke the Lambda function upon a new object creation (`s3:ObjectCreated:*`). |

---

## 💡 Technical Challenge & Solution (Proof of Skill)

The core technical achievement of this project was successfully stabilizing the environment by resolving recurrent integration errors between the Terraform AWS Provider and the LocalStack Docker container.

### 1. Module Refactoring

- **Before:** Monolithic structure (all resources in `main.tf`).
- **After:** Refactored into a reusable `modules/serverless_function` directory. The root `main.tf` now uses a clean **`module "s3_to_lambda_processor" { ... }`** block, demonstrating best practices for large-scale IaC management.

### 2. LocalStack Connection Fix

The deployment repeatedly failed with two critical errors:

- `Error: API error InvalidAccessKeyId`
- `Error: connect: connection refused`

**The solution required bypassing the AWS provider's default behavior and strictly synchronizing the shell environment:**

| **Configuration** | **Fix Implemented** | **Rationale** |
| --- | --- | --- |
| **Code (`main.tf`)** | Used `skip_credentials_validation = true`, `skip_requesting_account_id = true`. | Forced the AWS provider to accept mock credentials (`testing/testing`) and not contact the real AWS STS service. |
| **Shell Environment** | Explicitly exported `AWS_ENDPOINT_URL="http://localhost:4566"`. | Solved the `Unsupported argument` error in Terraform code by instructing the AWS provider via environment variable to target the LocalStack service instead of the public cloud. |
| **Docker Stability** | Implemented `docker stop` and `docker container prune` cleanup scripts. | Resolved the persistent `connection refused` and `port is already allocated` errors by ensuring LocalStack was running as a clean, sole process before Terraform attempted connection. |

---

## ⚙️ How to Run the Project Locally

### Prerequisites

1. **Terraform CLI**
2. **Docker Desktop** (must be running)
3. **AWS CLI** (for testing the final deployment)

### Deployment Steps

1. **Clone the Repository:**Bash
    
    `git clone [Your Repository URL]
    cd serverless-refactoring-project`
    
2. Start LocalStack Server (Terminal 1):Bash
    
    Open a separate terminal window/tab and start the local cloud environment.
    
    `docker run --rm -it -p 4566:4566 -p 4510-4559:4510-4559 localstack/localstack`
    
    *(Wait for the logs to show `Ready.`)*
    
3. Configure Environment (Terminal 2):Bash
    
    Switch to your main terminal and set the required variables. This step is mandatory.
    
    `export AWS_ACCESS_KEY_ID="testing"
    export AWS_SECRET_ACCESS_KEY="testing"
    export AWS_ENDPOINT_URL="http://localhost:4566"`
    
4. **Initialize and Apply:**Bash
    
    `terraform init -reconfigure
    terraform apply`
    

### Verification (Test the Trigger)

Once `terraform apply` is complete, verify the S3 $\rightarrow$ Lambda trigger by uploading a test file using the configured LocalStack endpoint:

Bash

`echo "Hello from Terraform" > test-data.txt
aws s3 cp test-data.txt s3://localstack-modular-trigger-bucket-2 --endpoint-url=http://localhost:4566`

![image.png](%F0%9F%9A%80%20Serverless%20Infrastructure%20Refactoring%20Project%20(T/image.png)