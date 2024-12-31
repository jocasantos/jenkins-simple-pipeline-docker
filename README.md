# A simple jenkins pipeline to verify if the docker slave configuration is working as expected.

### Create a AWS instance

1. Create a EC2 instance and connect to it using SSH.
> Already explain this part in another guide of mine. https://github.com/jocasantos/aws-nodejs-app-demo

### Install Java

1. Pre-Requisites:
    - Java (JDK)

2. Install Java
```bash
sudo apt update
sudo apt install openjdk-17-jre
```

3. Verify Java is Installed
```bash
java -version
```

### Install Jenkins
```bash
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```