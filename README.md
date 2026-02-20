# Building and deploying a Web App with CI/CD Deployment Leveraging AWS
In this project, i'll building from scratch a CI/CD pipeline that automates the build and deployment of a web app. This will be a devops project.

<img width="913" height="285" alt="Screenshot 2026-01-07 at 20 19 20" src="https://github.com/user-attachments/assets/cbb0d1d4-cf6c-43ac-9a97-ae1c6aa5f745" />

## Launch an EC2 Instance
<img width="1440" height="773" alt="Screenshot 2026-01-14 at 05 08 42" src="https://github.com/user-attachments/assets/601c84a6-a4c4-4226-a286-ff6c0308c6d6" />

## Connect to your EC2 Instance
To connect to my EC2 instance, you set the keypair file read/write permission using the command "chmod 400 devops-kp.pem". Then you copy the instnace public ip address. Then you connect to your EC2 instance using the command "ssh -i devops-kp.pem ec2-user@ip-address".

<img width="673" height="234" alt="Screenshot 2026-01-14 at 05 17 38" src="https://github.com/user-attachments/assets/e97c3312-c996-4c91-98a8-8ccfa68683a6" />

## Install Apache Maven and Amazon Corretto 8
1. Install Apache Maven using the commands below.
  - wget https://archive.apache.org/dist/maven/maven-3/3.5.2/binaries/apache-maven-3.5.2-bin.tar.gz
  - sudo tar -xzf apache-maven-3.5.2-bin.tar.gz -C /opt
  - echo "export PATH=/opt/apache-maven-3.5.2/bin:$PATH" >> ~/.bashrc
  - source ~/.bashrc
    
2. To verify that Maven is installed correctly, run the command
   - mvn -v

2. Next we're going to install Java 8, or more specifically, Amazon Correto 8. Run these commands below:
  - sudo dnf install -y java-1.8.0-amazon-corretto-devel
  - export JAVA_HOME=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64
  - export PATH=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64/jre/bin/:$PATH

4. To verify that you've installed Java 8 correctly, run this next:
  - java -version

## Create the Application
- Use mvn to generate a Java web app. To do this, use these commands:
  - mvn archetype:generate \
   -DgroupId=com.nextwork.app \
   -DartifactId=nextwork-web-project \
   -DarchetypeArtifactId=maven-archetype-webapp \
   -DinteractiveMode=false
<img width="817" height="265" alt="Screenshot 2026-01-14 at 05 35 57" src="https://github.com/user-attachments/assets/d2c9169a-417b-4975-9268-ecd048120057" />

## Connect VS Code with your EC2 Instance
- install the remote - ssh extension on VS code as shown in the image below.
  <img width="1440" height="634" alt="Screenshot 2026-01-14 at 06 01 35" src="https://github.com/user-attachments/assets/f9829ec8-5390-468e-adfc-25b1b190fe3c" />

## Install Git and clone project from this repo
By using the command below, you install git and clone your project inside your server succcess fully.
- sudo dnf update -y
- sudo dnf install git -y

  <img width="561" height="312" alt="Screenshot 2026-02-19 at 19 36 06" src="https://github.com/user-attachments/assets/2b36578c-19de-44f8-8f0f-604e195b6aa5" />
- git clone https://github.com/michealdayo64/Web-App-with-CI-CD-Pipeline.git

## Set Up AWS CodeArtifact
You create AWS CodeArtifact to store your code dependencies.

<img width="1440" height="820" alt="Screenshot 2026-02-19 at 19 45 29" src="https://github.com/user-attachments/assets/ac51e15f-b0ee-4af7-8df2-f93d944ca433" />

## Create AWS IAM Role and Policy
The goal of creating an IAM role in this project is to give permission to our EC2 to have access to our CodeArtifact.
### IAM Role
  <img width="1435" height="734" alt="Screenshot 2026-02-19 at 19 53 35" src="https://github.com/user-attachments/assets/fb4a1fc1-8267-435d-bc77-5a313f1dc01c" />

### IAM Policy
  
<img width="1439" height="806" alt="Screenshot 2026-02-19 at 19 55 08" src="https://github.com/user-attachments/assets/bb3bae45-21c5-40f5-9e2d-016d06fd2798" />

### EC2 Assume Role
<img width="1440" height="758" alt="Screenshot 2026-02-19 at 19 58 01" src="https://github.com/user-attachments/assets/7ac11d5a-84ac-4c44-88f5-d5cba60f26ad" />

## Connect CodeArtifact and Maven
- Here i connect my dependencies to my code CodeArtifact pulled Github
<img width="1006" height="781" alt="Screenshot 2026-02-19 at 20 02 48" src="https://github.com/user-attachments/assets/8a2608e7-2462-4139-8bc0-609f217f0cb1" />

- This when i compile my maven dependencies using the command "mvn compile -s settings.xml" which in turn store the dependencies on CodeArtifact.
<img width="1070" height="856" alt="Screenshot 2026-02-19 at 20 03 46" src="https://github.com/user-attachments/assets/663a248f-a8c2-48a7-a604-bda0a1c30020" />

- This is the display of the dependencies stored on the Repository created on CodeArtifact
<img width="1440" height="810" alt="Screenshot 2026-02-19 at 20 04 16" src="https://github.com/user-attachments/assets/f0595ca5-6fa8-4caf-8991-bcc56ae6bddc" />

## Set up CodeBuild Project
- Launch an S3 bucket to store your build artifacts.

<img width="1437" height="459" alt="Screenshot 2026-02-19 at 20 28 36" src="https://github.com/user-attachments/assets/546d8288-a7d0-4b0a-bf46-43e48c227fdc" />

- Set up a CodeBuild project which has a role that give CodeBuild access to CodeArtifact. You also store CodeArtifact dependencies for build on S3 Bucket
<img width="1440" height="805" alt="Screenshot 2026-02-19 at 20 41 59" src="https://github.com/user-attachments/assets/a74798dd-0efb-42ea-bfa5-36a522f52dd5" />

## Create buildspec.yml in your web app repository.
- Here the code in the buildspec.yml file help in compiling the project to a package

<img width="1384" height="626" alt="Screenshot 2026-02-19 at 21 01 28" src="https://github.com/user-attachments/assets/6337ae91-0856-45dc-91e8-310f9a7f8a1f" />
- Here is the result of our compiled code automated by CodeBuild

<img width="1440" height="757" alt="Screenshot 2026-02-19 at 21 32 00" src="https://github.com/user-attachments/assets/119954ce-924f-48d1-b900-3f4cfd0ba9a7" />

## Set up CodeDeploy
- Launch deployment instance with my CloudFormatio template. 
<img width="1439" height="754" alt="Screenshot 2026-02-19 at 21 42 17" src="https://github.com/user-attachments/assets/bbf4dcf7-ce9a-4a54-8c88-906cc46dbbc9" />


- Setting up my CodeDeploy application.
<img width="1440" height="319" alt="Screenshot 2026-02-19 at 21 44 56" src="https://github.com/user-attachments/assets/9e969abd-0de8-4d2e-a361-abb0c7cf95cb" />

- create a deployment group within our application to define how deployments will be done.

<img width="1440" height="814" alt="Screenshot 2026-02-19 at 21 57 25" src="https://github.com/user-attachments/assets/17e71db1-443d-4dee-b73b-faecba7434c4" />

- Launch a deployment!
<img width="1440" height="815" alt="Screenshot 2026-02-19 at 22 01 40" src="https://github.com/user-attachments/assets/450c839f-6d9c-441f-9c28-ae961068a334" />


## Set Up your Pipeline
- We'll start by setting up the basic pipeline structure and configuring its settings.
  


