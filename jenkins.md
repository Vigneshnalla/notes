# Jenkins Overview

Jenkins is a plain server that becomes powerful only when plugins are added. 

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