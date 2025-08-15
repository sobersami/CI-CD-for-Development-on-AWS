## 📌 Overview
This project demonstrates a full CI/CD pipeline on AWS to build, test, and deploy a Java web application automatically from GitHub to EC2 using:
1)	AWS CodePipeline (orchestrator)
2)	AWS CodeBuild (build automation)
3)	AWS CodeDeploy (deployment)
4)	AWS CodeArtifact (dependency management)
5)	Amazon S3 (artifact storage)
6)	Amazon EC2 (deployment target)
-	Whenever code is pushed to GitHub, the pipeline:
1)	Retrieves the source code
2)	Builds and packages the app
3)	Deploys it to the EC2 instance — zero manual intervention
________________________________________
## 🛠 Tech Stack
1)	Language: Java (JSP/Servlets)
2)	Build Tool: Maven
3)	AWS Services: EC2, CodePipeline, CodeBuild, CodeDeploy, CodeArtifact, S3, IAM
4)	Tools: GitHub, VS Code, SSH
________________________________________
## 📂 Project Structure
├── src/                  # Application source code
├── buildspec.yml          # CodeBuild build instructions
├── appspec.yml            # CodeDeploy deployment instructions
├── templates/             # CloudFormation templates
└── README.md              # Project documentation
________________________________________
## ⚡ Pipeline Flow
1)	Source → Fetches code from GitHub (webhook-enabled)
2)	Build → Compiles & packages app with CodeBuild (Maven + dependencies from CodeArtifact)
3)	Deploy → Pushes new build to EC2 via CodeDeploy
4)	Verify → Automated test by accessing the updated app
________________________________________
## Deployment Steps (High-Level)
---
## Set up Development Instance
1)	Launch Amazon Linux EC2
2)	Install Maven, Java, Git
3)	Clone the app repo from GitHub
## Configure CodeArtifact
1)	Create a Maven repo in CodeArtifact
2)	Link EC2 & CodeBuild to it via IAM policies
## Build Automation (CodeBuild)
1)	Create an S3 bucket for build artifacts
2)	Create a CodeBuild project linked to GitHub
## Deployment Automation (CodeDeploy)
1)	Launch EC2 target via CloudFormation
2)	Create CodeDeploy app & deployment group
## Orchestrate CI/CD (CodePipeline)
1)	Source → Build → Deploy stages
2)	Execution mode: Superseded (auto-cancels old runs)
## Test Automation
1)	Commit a small change
2)	Verify automatic redeployment to production
---








