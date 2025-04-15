## SONARQUBE - Java Code Analysis on AWS
🔧 Goal:
To analyze a Java Maven project using SonarQube hosted on AWS EC2 instances.

Step 1: Setup Java Maven Project on AWS
## Login to AWS Console:
Go to AWS Console

Sign in to your account

## Create EC2 Instance for Java Maven Project:
Name: java_mvn

AMI (OS): Ubuntu (Latest)

Instance Type: t2.micro (Free tier eligible)

Key Pair: Create or use existing

Security Group:

Allow SSH → Port 22

 Launch the instance.
 Connect to EC2 using SSH:

ssh -i "your-key.pem" ubuntu@<EC2-Public-IP>
 Setup Environment:
sudo apt update
java -version  

Check if Java is installed

If Java is not installed:

sudo apt install openjdk-11-jdk -y
Install Maven:

sudo apt install maven -y
Install Git (if not available):

git --version
 If not installed
sudo apt install git -y
✅ Clone your project:

git clone <your-repo-url>
cd java_project
✅ Build the Project:

mvn validate
mvn compile
mvn test
mvn package
A .war file will be generated inside the target/ directory.

# Step 2: Setup SonarQube Server on AWS

✅ Create Another EC2 Instance:
Name: Java_SonarQube

AMI (OS): Ubuntu (Latest)

Instance Type: t2.medium (SonarQube needs more memory)

Key Pair: Use the same or another

Security Group:

Allow SSH → Port 22

Allow Custom TCP → Port 9000 (For SonarQube Web UI)

✅ Connect to EC2:

ssh -i "your-key.pem" ubuntu@<SonarQube-EC2-Public-IP>
✅ Setup Java (Required for SonarQube):

sudo apt update
sudo apt install openjdk-11-jdk -y
✅ Install SonarQube:
Download SonarQube:


wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.4.1.88267.zip
Unzip the file:

sudo apt install unzip -y
unzip sonarqube-10.4.1.88267.zip
cd sonarqube-10.4.1.88267
Start SonarQube:

cd bin/linux-x86-64
./sonar.sh start
./sonar.sh status
If everything is good, SonarQube will be running.

# Step 3: Access SonarQube UI
Open your browser

Type: http://<SonarQube-EC2-Public-IP>:9000

# Step 4: Login & Generate Token
Login using:

Username: admin

Password: admin

Set a new password if asked

Navigate to:

My Account → Security → Generate Token

Copy the generated token (you’ll need it soon)

# Step 5: Analyze Java Code from First EC2 Instance
✅ On java_mvn instance:
Create a file named sonar-project.properties in the project root:


nano sonar-project.properties
Paste this content (replace values as needed):

properties

sonar.projectKey=my-java-project
sonar.projectName=My Java Project
sonar.projectVersion=1.0
sonar.sources=src
sonar.java.binaries=target
sonar.host.url=http://<SonarQube-EC2-Public-IP>:9000
sonar.login=<your-token-here>
Save the file.

Run SonarQube Scanner:

mvn sonar:sonar
This command will send your code to SonarQube server for analysis.

6️⃣ Step 6: View Results on SonarQube Dashboard
Go to browser: http://<SonarQube-EC2-Public-IP>:9000

Login and go to your project

🎉 You will see code analysis results, including:

Bugs

Vulnerabilities

Code smells

Duplications

Test coverage

