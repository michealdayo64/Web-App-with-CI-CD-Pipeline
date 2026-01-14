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



   
