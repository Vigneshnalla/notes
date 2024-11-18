## What is devops
- Devops is culture that helps organizations to improve the delivery of applications by ensuring proper automation with code  quality is maintained with in the applciation  and ensuring  continous monitoring and continous testing .By integrating these processes, DevOps allows teams to deliver applications to customers with high standards of reliability and performance.

## why devops
- Before DevOps, delivering an application to end users could take months, as various engineers—such as build and release engineers, test engineers, and others and also there is no proper communication betweent them 

  for suppose there is a hot fix in our application like the price of bus ticket  is  1000rs but instead of that it is showing  10 rs
  To prevent this, we need an immediate fix other wise we loss so much amount without devops it is not possible to sovle within days


DevOps solves this by enabling faster release cycles through CI/CD pipelines and allowing for rapid deployment of urgent fixes.



# Software Development Life Cycle (SDLC)

## Steps in SDLC

### 1. Planning
- Define project goals and objectives.
- Identify stakeholders and their requirements.
- Create a project plan, timeline, and budget.
- Assess feasibility and risks.

### 2. Defining Requirements
- Gather detailed user and system requirements.
- Document functional and non-functional requirements.
- Prepare a Software Requirements Specification (SRS) document.

#### **Functional Requirements**
- **User Authentication**:  
  The system must allow users to log in using a username and password. Users should also be able to reset their password via email.  
- **Payment Processing**:  
  The system must integrate with third-party payment gateways and generate invoices upon successful transactions.

#### **Non-Functional Requirements**
- **Performance**:  
  The system should handle up to 10,000 concurrent users, with a page load time of less than 2 seconds.
- **Scalability**:  
  The system should support horizontal scaling to handle growing user traffic.
- **Availability**:  
  The application must have 99.9% uptime with failover mechanisms in place.
- **Security**:  
  Data should be encrypted using AES-256, and all API endpoints must be secured using OAuth 2.0.

### 3. Designing
- Develop system and software architecture.
- Create wireframes, mockups, or prototypes.
- Define database schema, APIs, and integration points.
- Document technical specifications.

### 4. Building (Implementation)
- Write and develop the code based on the design.
- Follow coding standards and best practices.
- Perform version control and peer code reviews.
- Integrate modules for functionality.

### 5. Testing
- Conduct unit, integration, system, and acceptance testing.
- Identify and fix defects.
- Validate that the software meets requirements.
- Perform regression testing to ensure stability.

### 6. Deployment
- Deploy the software to the production environment.
- Monitor the deployment process for issues.
- Ensure a rollback plan is in place if needed.
- Hand over the system to the operations team.

### 7. Maintenance (Post-Deployment)
- Monitor system performance and availability.
- Fix bugs and implement updates as needed.
- Gather user feedback for improvements.
- Plan for future enhancements.



### DevOps Automation in SDLC Stages

1. **Building**
   - Automate code integration using **CI/CD pipelines** (e.g., Jenkins, GitHub Actions, GitLab CI).  
   - Use tools like **Maven** or **Gradle** for build automation.  
   - Implement containerization with **Docker** for consistent build environments.

2. **Testing**
   - Automate unit, integration, and functional tests with frameworks like **JUnit**, **Selenium**, or **Pytest**.  
   - Use CI pipelines to trigger automated test suites on every commit or build.  
   - Perform static code analysis using tools like **SonarQube** or **Checkmarx**.  

3. **Deploying**
   - Use **Infrastructure as Code (IaC)** tools like **Terraform** or **AWS CloudFormation** for automated provisioning.  
   - Deploy applications to environments using tools like **Ansible**, **Kubernetes**, or **Helm**.  
   - Implement **blue-green** or **canary deployments** for minimal downtime.

4. **Monitoring**
   - Automate monitoring with tools like **Prometheus**, **Grafana**, or **Datadog**.  
   - Set up alerts for critical metrics (e.g., CPU, memory, application latency) using **AWS CloudWatch** or **Nagios**.  

5. **Maintenance**
   - Use automated scripts for patch management and system updates.  
   - Continuously monitor logs using tools like **ELK Stack** (Elasticsearch, Logstash, Kibana) or **Splunk**.  
   - Leverage self-healing infrastructure to automatically restart failed instances or services.  



### Ways to interact with aws

#### AWS Management Console:
Web-based GUI for easy AWS resource management and monitoring.

####  AWS CLI:
Command-line tool for managing AWS services through terminal commands.

#### AWS SDKs:
Programming libraries for interacting with AWS services programmatically.

#### AWS CloudFormation:
Automates infrastructure provisioning using YAML/JSON templates. It is internal tool that means it is an aws tool it has more priority that terraform.

#### AWS CDK (Cloud Development Kit):
Defines AWS infrastructure using familiar programming languages.

#### Third-Party Tools (Terraform, Ansible, etc.):
Manages multi-cloud infrastructure with advanced orchestration.

#### AWS Systems Manager (SSM):
Remotely manages EC2 instances without SSH or RDP.

#### RESTful APIs:
Interact directly with AWS services using HTTPS requests.

=================================================================
Devpos Enginner primary focused on auotmations of building ,testing and deployment by writing automation scripts there by reducing manual intervention so that it will improve the efficiency and reduces the time for application deleivery

