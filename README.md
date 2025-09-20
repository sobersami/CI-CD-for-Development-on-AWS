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
<img width="1015" height="374" alt="image" src="https://github.com/user-attachments/assets/017b23c4-79c1-48fe-b41f-665d28e7ab06" />

________________________________________
## 🛠 Tech Stack
1)	Language: Java (JSP/Servlets)
2)	Build Tool: Maven
3)	AWS Services: EC2, CodePipeline, CodeBuild, CodeDeploy, CodeArtifact, S3, IAM
4)	Tools: GitHub, VS Code, SSH

## ⚡ Pipeline Flow
1)	Source → Fetches code from GitHub (webhook-enabled)
2)	Build → Compiles & packages app with CodeBuild (Maven + dependencies from CodeArtifact)
3)	Deploy → Pushes new build to EC2 via CodeDeploy
4)	Verify → Automated test by accessing the updated app
________________________________________
## Deployment Steps

## Set up Development Instance
1)	Launch Amazon Linux EC2
2)	Install Maven, Java, Git
3)	Clone the app repo from GitHub
<img width="743" height="175" alt="image" src="https://github.com/user-attachments/assets/87cfbf3c-d7fe-428b-8ea9-3106b20551c1" />
<img width="625" height="327" alt="image" src="https://github.com/user-attachments/assets/ce8a653c-1c8c-46a7-aebc-62015439379c" />
<img width="619" height="296" alt="image" src="https://github.com/user-attachments/assets/10cbe5af-d5e8-4a60-9b0d-52ce7baba1af" />
<img width="694" height="258" alt="image" src="https://github.com/user-attachments/assets/36e0eb49-3443-4afb-8796-236676cc8d52" />


## Configure CodeArtifact
<img width="462" height="267" alt="image" src="https://github.com/user-attachments/assets/25542acd-8e63-4b02-981c-e2bc7317cea8" />

1)	Create a Maven repo in CodeArtifact
2)	Link EC2 & CodeBuild to it via IAM policies(JSON) 
<img width="648" height="561" alt="image" src="https://github.com/user-attachments/assets/f670b608-4671-4a59-be75-b9d813e23a15" />

## Build Automation (CodeBuild)
1)	Create an S3 bucket for build artifacts
   <img width="487" height="281" alt="image" src="https://github.com/user-attachments/assets/18cc5ba5-029c-4fe5-8144-42169c66b07b" />
    <img width="852" height="572" alt="image" src="https://github.com/user-attachments/assets/cb85f428-a22c-4b3c-90ae-322b04b88a6c" />

2)	Create a CodeBuild project linked to GitHub
<img width="650" height="287" alt="image" src="https://github.com/user-attachments/assets/972c22c7-9ede-48d3-a672-ec474a3370b7" />

## Deployment Automation (CodeDeploy)
<img width="975" height="269" alt="image" src="https://github.com/user-attachments/assets/9f083cfb-bccd-4836-96f4-db63af05e3b3" />

1)	Launch EC2 target via CloudFormation
   <img width="502" height="347" alt="image" src="https://github.com/user-attachments/assets/e7c65293-b0dd-402b-92c2-e8d85f387dcb" />
   <img width="602" height="331" alt="image" src="https://github.com/user-attachments/assets/75511ca8-69c5-4aed-b857-a5b9a7b05906" />
2)	Create CodeDeploy app & deployment group
   
## Orchestrate CI/CD (CodePipeline)
1)	Source → Build → Deploy stages
   <img width="517" height="398" alt="image" src="https://github.com/user-attachments/assets/94160769-4339-431b-b77b-f2d407a04ac9" />
    <img width="2304" height="1464" alt="image" src="https://github.com/user-attachments/assets/014a8800-0b89-42cf-b557-0dd9980f362a" />

2)	Execution mode: Superseded (auto-cancels old runs)
   <img width="2304" height="1464" alt="image" src="https://github.com/user-attachments/assets/8e5811cf-c716-4b43-9605-6b99290efd8d" />
   <img width="2304" height="1464" alt="image" src="https://github.com/user-attachments/assets/d099e53d-f5c9-40ec-a13c-cf9391c47230" />

## Configuring the Source, Build, and Deploy Stages
  1) Under Connection, select your existing GitHub connection
<img width="2304" height="1464" alt="image" src="https://github.com/user-attachments/assets/1cc328ca-0951-4f01-afa1-e14ff9a4b877" />
<img width="2304" height="1464" alt="image" src="https://github.com/user-attachments/assets/51f53c28-c5ca-4f03-8902-bdc4992323d6" />
  2) Make sure that Webhook events is checked under Detect change events.
<img width="2304" height="1464" alt="image" src="https://github.com/user-attachments/assets/069038eb-8d4d-4352-a776-47d6ebc0d19d" />
  3) Select AWS CodeBuild from Other build providers in the Build provider dropdown.
     In the Build stage, your source code gets compiled and packaged into something that can be deployed.
     <img width="2304" height="1464" alt="image" src="https://github.com/user-attachments/assets/dcebb9ec-8b13-4bbb-aa41-1cfcb844f6bc" />
  4) Under Input artifacts, SourceArtifact should be selected by default.
     Input artifacts are the outputs from the previous stage that are used as inputs for the current stage. In our Build stage, we're using 
     <img width="2304" height="1464" alt="image" src="https://github.com/user-attachments/assets/d0c770d6-ccd5-4a2c-8d9f-fab3b322ab56" />
  5) In the Deploy provider dropdown, select AWS CodeDeploy.
     The Deploy stage is the final step in our pipeline. It's responsible for taking the application artifacts (the output from the Build stage) and deploying them to the target environment, which in our case is an EC2 instance.
In the Deploy provider dropdown, select AWS CodeDeploy.
  6) Check the box for Configure automatic rollback on stage failure.
<img width="2304" height="1464" alt="image" src="https://github.com/user-attachments/assets/4d617c9d-05ae-40e9-9d96-044128afac5a" />







## Test Automation

1)	Commit a small change
   <img width="2304" height="1464" alt="image" src="https://github.com/user-attachments/assets/5c8b3c4d-eeba-49b5-a428-0bf812952735" />
   <img width="2304" height="1464" alt="image" src="https://github.com/user-attachments/assets/57615ca9-207a-4673-8361-ace568b3cc95" />


2)	Verify automatic redeployment to production
   <img width="2304" height="1464" alt="image" src="https://github.com/user-attachments/assets/e237222e-ca36-410c-8304-ad69779020e3" />
   <img width="2304" height="1464" alt="image" src="https://github.com/user-attachments/assets/382f32bf-2d89-48d3-86ba-1bcb4300568a" />


## 🎯 Key Learning Outcomes

- Hands-on experience with AWS DevOps services

- Understanding of pipeline automation and infrastructure as code

- Skills to troubleshoot and monitor deployments









