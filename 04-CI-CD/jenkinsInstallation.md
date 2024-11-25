# Jenkins run on 8080 port
/var/lib/jenkins/secrets/initialAdminPassword

# Jenkins Installation Script

This script installs Jenkins on a Red Hat-based system.

## Step 1: Add the Jenkins repository
```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

## Step 2: Configure the Jenkins repository
```bash
cd /etc/yum.repos.d/
vim jenkins.repo
[jenkins]
name=Jenkins-stable
baseurl=http://pkg.jenkins.io/redhat-stable
gpgcheck=1
```

## Step 3: Import the GPG key
```bash
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

## Step 4: Install required packages
```bash
sudo yum install fontconfig java-17-openjdk -y
sudo yum install jenkins -y
```

## Step 5: Start and enable Jenkins service
```bash
sudo systemctl daemon-reload
sudo systemctl enable jenkins
sudo systemctl start jenkins
```




## node setup

# Jenkins Agent Installation Script for AGENT-1

This script installs the necessary components for setting up a Jenkins agent on AGENT-1.

## Steps

1. **Switch to root user**:  
   ```bash
   sudo su -
   ```

2. **Install required packages**:  
   ```bash
   sudo yum install fontconfig java-17-openjdk -y
   ```

3. **(Optional) Install Jenkins agent package**:  
   ```bash
   sudo yum install jenkins-agent -y
   ```

4. **(Optional) Start and enable Jenkins agent service**:  
   ```bash
   sudo systemctl start jenkins-agent
   sudo systemctl enable jenkins-agent
   ```


