# A simple jenkins pipeline to verify if the docker slave configuration is working as expected.

### Create a AWS instance

1. Create a EC2 instance and connect to it using SSH.
> Already explain this part in another guide of mine. https://github.com/jocasantos/aws-nodejs-app-demo

- For better practices you should store your `<name>.pem` file in your `~/.ssh` folder
- Then create a config file 
    ```bash
    vim config
    ```
- Edit the file
    ```bash
    Host jenkins
        Hostname instance-Public-IPv4-address
        User ubuntu
        IdentityFile ~/.ssh/<name>.pem
    ```
- Connect by SSH
    ```bash
    ssh jenkins
    ```

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
> By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

### Open port 8080

1. Go to aws console (website)

    - EC2 > Instances > Click on
    - In the bottom tabs -> Click on Security
    - Security groups
    - Add inbound traffic rules, allow custom TCP 8080 to 0.0.0.0/0 (all)

### Login to Jenkins using the below URL

1. http://ip-adress:8080 [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]

2. Run the command to copy the Jenkins Admin Password
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword   
```

3. Enter the password on your browser

4. Click on Install suggested plugins

5. Create First Admin User

6. Jenkins Installation is Successful. You can now starting using the Jenkins 🎉

### Install the Docker Pipeline plugin in Jenkins:

 - Log in to Jenkins.
 - Go to Manage Jenkins > Manage Plugins.
 - In the Available tab, search for "Docker Pipeline".
 - Select the plugin and click the Install button.
 - Restart Jenkins after the plugin is installed.

### Docker Slave Configuration

1. Run the below command to Install Docker
```bash
sudo apt update
sudo apt install docker.io
```

2. Grant Jenkins user and Ubuntu user permission to docker deamon.
```bash
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

3. Restart Jenkins (on your browser)
```bash
http://<ec2-instance-public-ip>:8080/restart
```

> The docker agent configuration is now successful! :tada: 