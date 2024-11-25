### Continuous Integration (CI)
Continuous Integration (CI) is a set of principles where code changes are automatically integrated into a shared repository. It helps us:
- Find and address bugs quicker.
- Make the process of integrating code across a team of developers easier.
- Improve software quality.
- Reduce the time it takes to release new feature updates.

### Continuous Delivery (CD)
After CI, the goal of Continuous Delivery (CD) is to automatically deliver code changes to staging environments, where they are tested and validated for production. This ensures that the application is always in a deployable state.

**Continuous Deployment** is a further step where every change that passes the tests is automatically deployed to production without manual approval.

### CI/CD Overview
CI/CD is the process of automating the steps from code commits made by developers to deploying the application to the server. It streamlines the entire development and deployment lifecycle, ensuring faster and more reliable releases.



## **Purpose**
CI/CD tools help streamline the process of building, testing, and deploying applications efficiently and reliably.

---

## **Key Features of CI/CD Tools**

1. **Unit Testing**  
   - Unit testing is nothing but testing code for a block or specific functionality..
   - Helps identify and fix bugs early in the development lifecycle.

2. **Static Code Analysis**  
   - Static code analysis is to verify format, all indentation is right, syntax is right, make sure u didn't declare any unnecessary variables.

3. **Code Quality / Vulnerability Checks**  
   - Code quality/ vulnerability testing is done to check for vulnerability, that can be misused by hackers.

4. **Automation**  
   -  Automation testing/functional testing /endtoend testing, in this you verify your application in end to end manner such that one functionality doesn't affect other functionality.

5. **Reports**  
   - reports for how many units test passed , what is code quality , build status etc.

6. **Deployment**  
   - Automates the deployment of applications to staging, production, or other environments.
   - Ensures consistency across environments.

---

Using GitHub Actions or GitLab pipelines for CI/CD can help save resources and increase efficiency.



Jenkins:
    we have to keep adding multiple machines(nodes) on that 
    you need to add more nodes if we are scaling
    we have to do a lot of lot of compute
    we have to add more vms
    
    so and also we not use these node everytime
    maintainece also becomes huge

we ahve to look fo rone thing autocaling and