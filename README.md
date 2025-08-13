# CI-CD-for-Development-on-AWS
---
## 💻Step 1
Set Up our Development Instance
- Let's launch an EC2 instance to develop our web app.
  
## Set up an IAM Admin User
1)	Create an IAM user with AdministratorAccess if you don't have one already.
2)	Log in to the AWS console using your IAM Admin User.

## Launch an EC2 Instance
 Launch an EC2 instance with the following configurations:
   1) AMI: Amazon Linux 2023
   2) Instance type: t2.micro
   3) Key pair: nextwork-keypair (Create a new one if needed and store the .pem file in a DevOps folder on your Desktop)
   4) Network settings:
     - Allow SSH traffic from: My IP

## Install VS Code
1)	Install VS Code if you haven't already.
2)	Open VS Code and open a new terminal.
3)	Navigate to the DevOps folder in your terminal: cd ~/Desktop/DevOps (Mac/Linux) or cd C:\Users\YourUserName\Desktop\DevOps (Windows).
4)	Change permissions of your .pem file:
5)	Mac/Linux: chmod 400 nextwork-keypair.pem
6)	Windows:
    <img width="516" height="141" alt="image" src="https://github.com/user-attachments/assets/88f82cc5-d37a-4f15-869e-1ac497a9a37e" />

## Set up our EC2 Instance
1)	Connect to your EC2 instance using SSH from your VS Code terminal.
2)	Install Maven, Java and Git on your EC2 instance by running these commands :
   <img width="619" height="296" alt="image" src="https://github.com/user-attachments/assets/8021a670-420f-4f80-b68a-e22a5433ef62" />
- Verify installations:
1)	mvn -v (check for Maven version 3.5.x)
2)	java -version (check for Java 8)
3)	git -v
## Create the Application
1) Generate a Java web app using Maven:
   <img width="694" height="258" alt="image" src="https://github.com/user-attachments/assets/99a62b01-1b70-4ef3-8530-e032bffe25aa" />
## Connect VS Code with your EC2 Instance
1)	Connect VS Code to your EC2 instance using the Remote-SSH extension.
2)  Open the nextwork-web-project folder in VS Code on your EC2 instance.






