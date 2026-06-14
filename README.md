\# DevOps Project 01 — Java App CI/CD Pipeline



\## Tools Used

\- Jenkins, Maven, Docker, Terraform, AWS EC2, Git



\## Pipeline Flow

Git Push → Jenkins → Maven Build → Docker Image → Deploy to EC2



\## How to Run

1\. Provision EC2: `cd terraform \&\& terraform apply -var="key\_name=devops-key"`

2\. Start Jenkins: `docker compose up -d`

3\. Run pipeline from Jenkins UI



