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

# Build Options in Jenkins

Jenkins provides various build triggers to automate the build process based on specific conditions. Here are the key build options available:

### 1. Build Periodically
This trigger allows you to schedule builds at specific times using cron-like syntax. You can set it to run builds at regular intervals, such as daily, weekly, or at specific hours.

### 2. Poll SCM
This trigger periodically checks the source code management (SCM) system for changes. Jenkins polls the repository at specified intervals and triggers a build if it detects any changes since the last build.

### 3. GitHub Hook Trigger for GIT SCM Polling
This trigger initiates a build when GitHub sends a webhook notification to Jenkins. It is triggered by events on the GitHub repository, such as push events or pull request updates.

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


## Configuring Jenkins UI

1. **Go to Jenkins UI**:
   - Open your Jenkins instance in a web browser.

2. **Configure Your Job**:
   - Navigate to the job you want to configure and click on **Configure**.

3. **Build Triggers**:
   - Under the **Build Triggers** section, check **GitHub hook trigger for GITScm polling**.
   - Save your configuration.



## Throttle Builds in Jenkins

In Jenkins, **throttling builds** helps manage system resources effectively by controlling the number of concurrent builds that run simultaneously. This ensures that no single job or node consumes excessive resources, leading to a more balanced and efficient build environment.

### Throttle Build Options

1. **Throttle Concurrent Builds**  
   This option allows you to limit the total number of builds that can run concurrently across all nodes. By setting this limit, you can prevent system overload from too many builds running at once, which helps maintain optimal performance.

2. **Throttle Builds per Node**  
   With this option, you can specify the number of builds allowed to run concurrently on a single node. This setting distributes resource usage evenly across the Jenkins environment, ensuring no individual node is overwhelmed by too many builds.



# Master-Slave Architecture in Jenkins

The Master-Slave Architecture in Jenkins is a way to distribute the workload
of building, testing, and deploying applications across multiple "nodes". The "Master" is the main jenkins server and slaves or agents are additional machines that master controls

### 1. Master (Controller)
- The master node is responsible for managing the Jenkins environment, including:
  - Scheduling builds
  - Handling configurations and plugin management
  - Managing the user interface, job queues, and job configurations
- While the master can run jobs, it’s  preferred to delegate jobs to slave nodes to keep the master focused on coordination.

### 2. Slave (Agent)
- Slaves, or agents, are separate machines configured to execute jobs as directed by the master.
- Agents can run on various operating systems, allowing jobs to be executed on different environments.
- Each agent can run one or more jobs simultaneously, based on its configuration and capacity.

In slave we don't install jenkins, which only requires Java on the agent machine

## Security Configuration in Jenkins

To configure security settings in Jenkins, follow these steps:

### Navigate to Security Settings:
- Go to **Manage Jenkins > Configure Global Security**.

### User Access Administration:
- Under **Authorization**, configure user access levels:
  - **Matrix-based Security**: Allows fine-grained control by setting specific permissions for each user or group.

However, matrix-based security only provides limited filtering and isn’t ideal for managing access per project. To address this, you can install the **Role-Based Authorization Strategy** plugin.

## Setting Up Role-Based Authorization After Adding Role-Based Authorization Strategy Plugin
1. **Install the Role-Based Authorization Strategy Plugin**:
   - This plugin adds new options in **Configure Global Security** to assign roles based on user access needs.

2. **Configure Role-Based Strategy**:
   - Go to **Manage Jenkins > Configure Global Security** and select **Role-Based Strategy** under Authorization.
   
3. **Assign Roles**:
   - Navigate to **Manage and Assign Roles**. Define roles as needed:
     - **Global Roles**: Set general roles, such as "Developer."
     - **Item Roles**: Use these for project-specific filtering. Example: `portal.*` for a pattern-based project role.

4. **Assigning Roles to Users**:
   - In **Assign Roles**, grant users the required **Global Roles** (required to access the Jenkins console) and any **Item Roles** specific to their projects.

#### Reference Video
For a visual guide, refer to this video tutorial:
[Watch Tutorial](https://www.youtube.com/watch?v=XuW2UHsSliU&list=TLPQMDIxMTIwMjRLuoK1kw6-4w&index=4)


### Deploying Maven Application to Tomcat Using Jenkins Pipeline

#### Step 1: Configure Jenkins Credentials
First, add the Tomcat user credentials in Jenkins global credentials.

#### Step 2: Install Required Plugins
You need to install the following plugins:
- **Maven Integration Plugin**
- **Deploy to Container Plugin**  
  This plugin allows you to deploy a WAR file to a container after a successful build.

#### Step 3: Create a New Item
1. **Select "New Item"** in Jenkins.
2. **Choose "Maven Project".**
3. **Configure Source Code Management**:
   - Add your Git repository.

#### Step 4: Configure Build Settings
1. **In the "Build" section**, specify the goals and options:
   ```plaintext
   clean install package
   ```
2. **In the "Post-build Actions" section**, select:
   - **Deploy to Container**
     - Specify the WAR file path:
       ```plaintext
       **/*.war
       ```

#### Step 5: Add Container Configuration
1. **Add Container**:
   - Select your Tomcat version.
   - Provide the Tomcat URL (e.g., `http://your-tomcat-url:8080`).
   - Select the previously configured credentials.

#### Step 6: Save Configuration
Click on **Apply** and then **Save** your project configuration.

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

### ----------------- General Things --------------------------
df -ht
it is using /tmp diskspace issue

we can configure monitoring 

By using jekkins url /8080/restart
we can restart the serivce

SystemLevel Variable:
   we can use this variable across multiple jobs
   we add this by go to manage Jenkins --> System --> Gloabal Properties --> env variables and add 

Jenkisn Variables: built in varibales
   http: jenkins url /env-vars.html
      sucha as build_number, job_name

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





