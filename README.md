# DevOps Project 01 — Java App CI/CD Pipeline

## Overview
End-to-end CI/CD pipeline for a Java Spring Boot web application deployed on AWS.

## Architecture
![Pipeline Flow](https://i.imgur.com/placeholder.png)

## Tools & Technologies
| Tool | Purpose |
|------|---------|
| Jenkins | CI/CD Pipeline |
| Maven | Build Java application |
| Docker | Containerization |
| Docker Hub | Container Registry |
| Terraform | Infrastructure as Code |
| AWS EC2 | Application Hosting |
| Git/GitHub | Version Control |

## Pipeline Stages
1. Jenkins pulls code from GitHub
2. Maven builds the WAR file
3. Docker builds image from WAR
4. Image pushed to Docker Hub
5. EC2 pulls image and runs container

## Infrastructure
- EC2 t3.micro (Free Tier)
- Security Group (ports 22, 8080)
- Provisioned with Terraform

## Project Structure
├── Dockerfile          # App container definition

├── Jenkinsfile         # CI/CD pipeline stages

├── docker-compose.yml  # Jenkins setup

├── plugins.txt         # Jenkins plugins

└── terraform/

└── main.tf         # AWS infrastructure

## How to Run

### 1. Provision AWS Infrastructure
```bash
cd terraform
terraform init
terraform apply -var="key_name=YOUR_KEY"
```

### 2. Start Jenkins
```bash
docker compose up -d
# Open http://localhost:8080
```

### 3. Configure Jenkins
- Add AWS credentials
- Add EC2 SSH key
- Add Docker Hub credentials
- Create pipeline job pointing to this repo

### 4. Run Pipeline
Click **Build Now** — pipeline runs automatically on every push.

## Author
Mohamed Ashraf Abdullah
- Junior DevOps Engineer
- Cairo, Egypt



