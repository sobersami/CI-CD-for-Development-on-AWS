# CI-CD-for-Development-on-AWS
---
## 💻Step 1
Set Up our Development Instance
- Let's launch an EC2 instance to develop our web app.
  
## 1. Set up an IAM Admin User
1)	Create an IAM user with AdministratorAccess if you don't have one already.
2)	Log in to the AWS console using your IAM Admin User.

## 2. Launch an EC2 Instance
 Launch an EC2 instance with the following configurations:
   1) AMI: Amazon Linux 2023
   2) Instance type: t2.micro
   3) Key pair: nextwork-keypair (Create a new one if needed and store the .pem file in a DevOps folder on your Desktop)
   4) Network settings:
     - Allow SSH traffic from: My IP

## 3. Install VS Code
1)	Install VS Code if you haven't already.
2)	Open VS Code and open a new terminal.
3)	Navigate to the DevOps folder in our terminal: cd ~/Desktop/DevOps (Mac/Linux) or cd C:\Users\YourUserName\Desktop\DevOps (Windows).
4)	Change permissions of your .pem file:
5)	Mac/Linux: chmod 400 nextwork-keypair.pem
6)	Windows:
    <img width="516" height="141" alt="image" src="https://github.com/user-attachments/assets/88f82cc5-d37a-4f15-869e-1ac497a9a37e" />

## 4. Set up our EC2 Instance
1)	Connect to your EC2 instance using SSH from your VS Code terminal.
2)	Install Maven, Java and Git on your EC2 instance by running these commands :
   <img width="619" height="296" alt="image" src="https://github.com/user-attachments/assets/8021a670-420f-4f80-b68a-e22a5433ef62" />
---
 -  Verify installations:
1)	mvn -v (check for Maven version 3.5.x)
2)	java -version (check for Java 8)
3)	git -v
---
## 5. Create the Application
1) Generate a Java web app using Maven:
   <img width="694" height="258" alt="image" src="https://github.com/user-attachments/assets/99a62b01-1b70-4ef3-8530-e032bffe25aa" />
---
## 6. Connect VS Code with your EC2 Instance
1)	Connect VS Code to your EC2 instance using the Remote-SSH extension.
2)  Open the nextwork-web-project folder in VS Code on your EC2 instance.
3)  Modify index.jsp in src/main/webapp with your name.
---
## 7. Add, commit, and push your code to GitHub
          <img width="958" height="250" alt="image" src="https://github.com/user-attachments/assets/9a0aac99-6169-4139-adc4-8f9bbb5d2e8f" />

1)	Add, commit, and push your code:
2) 	Use a personal access token (PAT) instead of your password when prompted for credentials.
3)	Verify your code is in your GitHub repository.
---
## 📦 Step 2
## Set up CodeArtifact Repository
- Set up AWS CodeArtifact to manage your web app's packages.
---
NOW,
## 1. Create a CodeArtifact repository with: 
1)	Repository name: nextwork-devops-cicd
2)	Package format: Maven
3)	Enable public upstream repositories: Maven Central
4)	Domain name: nextwork (or use existing)
5)	Get connection instructions for Maven.
---
## 2. Create IAM policy and role for CodeArtifact access.
1)	Create an IAM policy named codeartifact-nextwork-consumer-policy with the following JSON:
   <img width="648" height="561" alt="image" src="https://github.com/user-attachments/assets/e7295388-b842-4252-bc23-4142bd45c131" />

3)	Create an IAM role named ec2-instance-nextwork-cicd for EC2, and attach the codeartifact-nextwork-consumer-policy.
4)	Attach the ec2-instance-nextwork-cicd IAM role to your EC2 instance
---
## 3. Connect CodeArtifact and Maven.
1)	Export CodeArtifact auth token in your EC2 terminal using the command from connection instructions.
2)	Create settings.xml in your project root directory.
3)	Copy and paste steps #4-6 from CodeArtifact connection instructions into settings.xml within <settings> tags.
4)	Build your project using Maven: mvn compile -s settings.xml.
5)	Verify packages in your CodeArtifact repository.
---
## 🛠️ Step 3

Set up AWS CodeBuild to automate the build process.
## 1. Create an S3 bucket to store CodeBuild artifacts.
1)	Navigate to the S3 service in the AWS Console.
2)	Create an S3 bucket named nextwork-devops-cicd-.
## 2. Create a CodeBuild project.
1)	Navigate to the CodeBuild service in the AWS Console.
2)	In the CodeBuild console, click Create build project.
3)	On the Create build project page, configure the following: Project name: nextwork-devops-cicd
---
-	Source
•	In the Source section:
1)	Source provider: Select GitHub.
2)	Repository: Choose our repository (<our_github_username>/nextwork-web-project).
   <img width="511" height="298" alt="image" src="https://github.com/user-attachments/assets/752472f1-bfd5-48a7-a2db-e2112efdb9fd" />

---
-	Configure Environment
In the Environment section:
1)	Environment image: Select Managed image.
2)	Operating system: Choose Amazon Linux 2.
3)	Runtime: Choose Standard.
4)	Image: Choose aws/codebuild/amazonlinux2-x86_64-standard:coretto8.
5)	Image version: Select Always use the latest image for this runtime version.
---
- In the Service role section, let's keep the default and let CodeBuild create a New service role.
- In the Buildspec section, select Use a buildspec file. This tells CodeBuild to use a buildspec.yml file in your repository to define the build commands.
---
-	In the Artifacts section:
1)	Artifact type: Select Amazon S3.
2)	Bucket name: Enter your S3 bucket name nextwork-devops-cicd. You'll need to create this bucket if you don't have it yet!
3)	Name: nextwork-devops-cicd-artifact
4)	Artifacts packaging: Select zip.
---
-	In the Logs section:
1)	Enable CloudWatch logs if they're not enabled yet.
2)	Set the CloudWatch group name to /aws/codebuild/nextwork-devops-cicd
•	Click Create build project.
---
## 3.  Update CodeBuild IAM role .
1)	Navigate to the IAM console.
2)	Attach the codeartifact-nextwork-consumer-policy to the CodeBuild service role (e.g., codebuild-nextwork-devops-cicd-service-role)
---
1.	Create buildspec.yml in your project root directory.
- Create buildspec.yml with the following content (replace placeholders)
- Commit and push changes to GitHub
  <img width="545" height="473" alt="image" src="https://github.com/user-attachments/assets/7cef0705-8450-4f8f-9d8f-442a4d3b4df4" />
---
2.	Test your CodeBuild project.
- Start a build manually in the CodeBuild console for the nextwork-devops-cicd project.
- Monitor the build logs for success.
- Verify the build artifact in your S3 bucket (nextwork-devops-cicd-enter our name ).
---

# Step 4
- Set up CodeDeploy
Let's set up AWS CodeDeploy to automate the deployment of our built web application to our EC2 instance. CodeDeploy will ensure that our application updates are deployed smoothly and efficiently.

---
In this step, you're going to:
•	Launch a CloudFormation stack for our deployment instance.
•	Set up a CodeDeploy application and deployment group.
•	Configure a CodeDeploy service role.
•	Successfully deploy our web app!

---
---
<img width="975" height="269" alt="image" src="https://github.com/user-attachments/assets/1e59243f-d98b-4923-acfd-d7ecfd2cf376" />

---
 ## 1.	Launch your deployment infrastructure
 -	Launch a new stack with this template: nextwork-web-app.yaml template file.
 -  It might take a few minutes until the stack status becomes CREATE_COMPLETE
## 2.	Create CodeDeploy Application
Let's automate deployments with CodeDeploy!
 - Open the CodeDeploy console.
 -	Create a new application called nextwork-devops-cicd
## 3. Create Deployment Group
Now, let's create a deployment group within our application to define how deployments will be done.
1)	Create a deployment group called nextwork-devops-cicd-deployment-group
2)	Set up a AWSCodeDeployRole role for the deployment group.
3)	Configure your deployment group to deploy to your deployment instance, which has the tag:
a.	Key: role
b.	Value: webserver.
4)	Uncheck Enable load balancing to avoid charges!
## 4. Create Deployment
1)	Create a deployment with the build artifact in your S3 bucket, using the deployment's revision location.











