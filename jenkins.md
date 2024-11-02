# Jenkins Overview

Jenkins is a plain server that becomes powerful only when plugins are added. 

A plugin in jenkisn is an add-on that extends the functionaliy of the jenkins software

## Adding Nodes for Load Distribution

We can add nodes to distribute the load for building multiple projects according to different environments. 

### Remote Root Directory for Logging

Ensure to configure the remote root directory for logging.

## Launch Methods

1. **Launch Agent by Connecting to the Controller**
2. **Launch Agent via SSH**

## Steps to Configure Webhook in GitHub

1. **Go to Your Repository**
2. **Go to Settings**
3. **Select Webhooks**
4. **Add Webhook**
5. **Configure the Webhook**:
   - **Payload URL**: `http://<jenkins-agent-url>:8080/github-webhook/`
     - Replace `<jenkins-agent-url>` with the actual URL of your Jenkins agent.
   - **Content type**: Select `application/json`.
   - **Secret**: (Optional) If you have configured a secret in Jenkins for security, enter it here.
   - **Which events would you like to trigger this webhook?**: 
     - Select **Just the push event** or **Let me select individual events** (you can select multiple events like Pull Requests, Push, etc.).
   - **Active**: Ensure the webhook is marked as **Active**.

6. **Save the Webhook**:
   - Click the **Add webhook** button to save your settings.


***IN NOde***
## Configuring Jenkins UI

1. **Go to Jenkins UI**:
   - Open your Jenkins instance in a web browser.

2. **Configure Your Job**:
   - Navigate to the job you want to configure and click on **Configure**.

3. **Build Triggers**:
   - Under the **Build Triggers** section, check **GitHub hook trigger for GITScm polling**.
   - Save your configuration.
--------------------------------------------


I automated the backend process by setting up CI/CD pipelines in Jenkins. First, I configured Jenkins, a Jenkins agent, and a Nexus server. I created a folder called "expense" in Jenkins, with two pipelines. The first pipeline (upstream job) pulls the backend code, navigates into the appropriate folder, takes the version from package.json, runs npm install, creates a zip file, and uploads it to Nexus. It then triggers the second pipeline (downstream job) by passing the version number.

The downstream job handles deploying the backend instance with the latest code. It reads the version parameter and uses Terraform to provision resources by running terraform apply -var="appversion={params.appVersion}". Then, using remote execution, it passes the version to Ansible, which attempts to connect to the backend instance. However, the backend instance is in a security group that doesn’t allow SSH access from the Jenkins agent (due to restrictions from AWS tools). To resolve this, I had to add an inbound rule in the backend's security group to allow SSH access from all private instances within the default VPC CIDR, enabling the Jenkins agent to connect and deploy the code successfully.

After Ansible connects to the backend instance, it uses the passed version to download the corresponding code from the Nexus server. Ansible then places the zip file containing the source code in the backend instance and extracts it, ensuring the backend is running the latest version of the code.


### Jenkins Slowness and Build issues in Cloud
when we stop and start the instance it is becoming slow for navigation from one palc eot another palce

Manage Jenkins:
   System:
      Jenkins Url:
         update new public ip adress

   we can alos give 0.0.0.0

When you run multiple jobs in Jenkins, specifying paths for tools and dependencies in every job can be cumbersome and error-prone. To streamline this process, you can configure tools globally in Jenkins, allowing you to reference them in your jobs without needing to specify the path each time.

Global Tool Configuration:

In Jenkins, go to Manage Jenkins > Global Tool Configuration.
Here, you can configure various tools (like JDK, Maven, Ant, etc.) and their installations.
For each tool, you can specify the path or let Jenkins automatically manage it.
Using Tool Configurations in Jobs:

When configuring a job, you can refer to the tools you've set up in the Global Tool Configuration.
For example, instead of hardcoding the path to Maven in every job, you can select the Maven installation from the dropdown in the job configuration.
Advantages:

Consistency: Ensures that all jobs use the same version of the tools.
Maintenance: If you need to change the path or version of a tool, you only need to update it in one place.
Simplicity: Reduces complexity in job configurations, making them easier to read and maintain.

----------------- General Things --------------------------
df -ht
it is using /tmp diskspace issue

we can configure monitoring 

By using jekkins url /8080/restart
we can restart the serivce

-------------------------------------------
pipeline {
    agent any

    environment {
        AWS_CREDENTIALS_ID = 'aws-credentials' // Use the ID you provided when adding credentials
        REGION = 'us-west-2' // Set your desired AWS region
        ACCOUNT_ID = '123456789012' // Your AWS account ID
        APP_VERSION = 'latest' // Set your application version
    }

    stages {
        stage('Docker build') {
            steps {
                script {
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: AWS_CREDENTIALS_ID]]) {
                        sh """
                            aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com
                            docker build -t ${ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/expense-backend:${APP_VERSION} .
                            docker push ${ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/expense-backend:${APP_VERSION}
                        """
                    }
                }
            }
        }
    }
}





