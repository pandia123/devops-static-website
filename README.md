# DevOps Static Website Deployment on AWS

## Project Overview

This project demonstrates an end-to-end DevOps workflow for deploying a static website using AWS cloud services, Docker containers, Infrastructure as Code (Terraform), and AWS-native CI/CD services.

The website source code is hosted on GitHub, containerized using Docker, stored in Amazon Elastic Container Registry (ECR), and automated through AWS CodeBuild.

## Technologies Used

### Cloud Services

* AWS ECR (Elastic Container Registry)
* AWS CodeBuild
* AWS VPC
* AWS IAM
* AWS S3

### DevOps Tools

* Git
* GitHub
* Docker
* Terraform
* AWS CLI

### Programming & Configuration

* HTML
* CSS
* JavaScript
* YAML
* HCL (Terraform)

---

## Project Architecture

```text
GitHub Repository
       │
       ▼
AWS CodeBuild
       │
       ▼
Docker Image Build
       │
       ▼
Amazon ECR
       │
       ▼
Container Image Storage
```

Infrastructure Provisioning:

```text
Terraform
    │
    ▼
AWS VPC
AWS Subnets
AWS Internet Gateway
AWS Route Tables
AWS ECR
AWS CodeBuild
```

---

## Features

* Infrastructure provisioning using Terraform
* Dockerized static website deployment
* Automated container image build process
* Amazon ECR integration
* AWS CodeBuild integration
* Version-controlled infrastructure
* Modular Terraform structure
* Cost-optimized AWS architecture

---

## Repository Structure

```text
devops-static-website/
│
├── terraform/
│   ├── ecr/
│   ├── networking/
│   └── codebuild/
│
├── Dockerfile
├── buildspec.yml
├── index.html
├── timer.html
├── tooplate-mochi-script.js
├── tooplate-mochi-space.css
└── README.md
```

---

## Infrastructure Components

### Networking

Terraform provisions:

* Custom VPC
* Public Subnet 1
* Public Subnet 2
* Internet Gateway
* Route Tables
* Route Table Associations

### Container Registry

Terraform provisions:

* Amazon ECR Repository

### Continuous Integration

AWS CodeBuild:

* Pulls source code from GitHub
* Builds Docker image
* Tags Docker image
* Pushes image to Amazon ECR

---

## Docker Workflow

Build Docker Image

```bash
docker build -t devops-static-website:v1 .
```

Run Container

```bash
docker run -d -p 8080:80 devops-static-website:v1
```

Access Application

```text
http://localhost:8080
```

---

## Terraform Deployment

Initialize Terraform

```bash
terraform init
```

Review Changes

```bash
terraform plan
```

Apply Infrastructure

```bash
terraform apply
```

Destroy Infrastructure

```bash
terraform destroy
```

---

## CI/CD Workflow

1. Developer pushes code to GitHub.
2. AWS CodeBuild pulls source code.
3. Docker image is built automatically.
4. Image is pushed to Amazon ECR.
5. Updated image becomes available for deployment.

---

## Key Learnings

* Infrastructure as Code using Terraform
* Docker containerization
* Amazon ECR image management
* AWS CodeBuild automation
* VPC networking concepts
* Git and GitHub workflows
* CI/CD implementation on AWS
* Cloud resource management and cost optimization

---

## Future Enhancements

* AWS CodePipeline integration
* Amazon EKS deployment
* Kubernetes manifests
* AWS Lambda notifications
* CloudFormation templates
* Monitoring with CloudWatch
* Automated deployment pipeline

---

## Author

Bajarang Pandia

Aspiring DevOps Engineer | AWS | Docker | Terraform | CI/CD | Cloud
