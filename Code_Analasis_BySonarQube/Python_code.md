## Python Code Analysis by integrating_SonarQube

This document provides a step-by-step guide to set up SonarQube for analyzing a Python project using two AWS EC2 instances.

##Step 1: Create EC2 Instance for Python Project

Instance Name: Python_server

OS: Ubuntu (latest version)

Instance Type: t2.micro

Key Pair: Choose or create one

Security Group:

Allow SSH (22)

Network: Default or custom

Storage: Default

Launch the instance after configuration.

Step 2: Connect to Python Server and Set Up Environment

Connect to the instance:

ssh -i "<your-key>.pem" ubuntu@<Python_Server_Public_IP>

Update the system:

sudo apt update

Clone the project repository:

git clone https://github.com/xaravind/Documentation.git
cd Documentation/python code

Download and install SonarScanner:

wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
sudo apt install unzip
unzip sonar-scanner-cli-5.0.1.3006-linux.zip
sudo mv sonar-scanner-5.0.1.3006-linux /opt/sonar-scanner
echo 'export PATH=$PATH:/opt/sonar-scanner/bin' >> ~/.bashrc
source ~/.bashrc

Step 3: Create EC2 Instance for SonarQube

Instance Name: SonarQube_server

OS: Ubuntu (latest version)

Instance Type: t2.medium

Key Pair: Use the same or another

Security Group:

Allow SSH (22)

Allow Custom TCP (9000)

Network: Default or custom

Storage: Default

Launch the instance after configuration.

Step 4: Set Up SonarQube Server

Connect to the SonarQube instance:

ssh -i "<your-key>.pem" ubuntu@<SonarQube_Server_Public_IP>

Update the system:

sudo apt update

Install Java JDK:

sudo apt install openjdk-17-jdk

Download and set up SonarQube:

wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-25.4.0.105899.zip
sudo apt install unzip
unzip sonarqube-25.4.0.105899.zip
cd sonarqube-25.4.0.105899/bin/linux-x86-64/
./sonar.sh start
# Optional: Check status
./sonar.sh status

Step 5: Access SonarQube Web Interface

Open a browser in Incognito Mode.

Visit:

http://<SonarQube_Public_IP>:9000

Login with default credentials:

Username: admin

Password: admin

Change the password when prompted.

Create a new project and generate a token.

Step 6: Run SonarScanner from Python Server

Go back to the Python Server.

Run the following command using your project key and token:

sonar-scanner \
  -Dsonar.projectKey=Python_project \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://<SonarQube_Public_IP>:9000 \
  -Dsonar.token=<Generated_Token>

✅ If successful, you'll see EXECUTION SUCCESS.

Step 7: View Analysis Results

Go back to the SonarQube Web UI.

Open your project dashboard.

Review analysis results: bugs, vulnerabilities, code smells, and more.

✅ SonarQube integration for Python code analysis is now complete!

