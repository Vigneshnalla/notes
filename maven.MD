# What is Maven

- **Maven** is a build automation and project management tool primarily used in Java-based projects. It is free, open-source, and maintained by Apache.
- It simplifies the build process by providing a standardized way to manage project dependencies, compile code, run tests, and package the application for deployment.
- Maven uses a configuration file called `pom.xml` (Project Object Model), where you define project dependencies, plugins, and other project details.
- It automates the process of downloading necessary libraries from a central repository, making it easier to manage project dependencies.

In most projects, development doesn't start from scratch—Maven offers a wide range of plugins to assist with various tasks.


### Instead of developing line by line, developers can directly use already developed modules and libraries, accelerating the development process.
## Build Tools by Language

- **Java**: Maven, Gradle
- **.NET**: MSBuild
- **JavaScript/TypeScript**: npm
- **Python**: setuptools, PyBuilder
- **C/C++**: Make
- **PHP**: Composer




# What is a .JAR File?

- **JAR** stands for **Java ARchive**. It is a package file format used to bundle multiple Java class files, along with associated metadata and resources (like images, configuration files, etc.), into a single file for distribution.
- A JAR file includes the compiled `.class` files of a Java project.

First we convert java files to .class files and then we combine all the .class files into a .jar file
# What is a .WAR File?

- **WAR** stands for **Web Application Archive**. It is a file used to package a Java-based web application for deployment on a web server or an application server like Apache Tomcat, JBoss, or GlassFish.
- A WAR file includes all the components of a web application, such as servlets, JavaServer Pages (JSPs), HTML files, JavaScript files, and images.

myapp.war
|-- META-INF/
| |-- MANIFEST.MF
|-- WEB-INF/
| |--classes/
| | |-- com/
| | |-- example/
| | |-- MyServlet.class
| |--lib/
| | |--some-library.jar
| |--web.xml
|-- static/
| |-- css/
| |--js/
| |--images/
|-- index.html

# Maven Goals

1. **Validate**
   - Command: `mvn validate`
   - Description: Executes where `pom.xml` is available to ensure the project structure is correct.

2. **Compile**
   - Command: `mvn compile`
   - Description: Compiles the code located in `src/main/java`, creating two new directories:
     - `target`
     - `classes`
     - Output: The compiled classes (e.g., `App.class`) are generated in `target/classes`.

3. **Test**
   - Command: `mvn test`
   - Description: Runs tests using a testing framework like JUnit.

4. **Package**
   - Command: `mvn package`
   - Description: Packages the compiled code into a JAR or WAR file, stored inside the `target` directory.

5. **Verify**
   - Command: `mvn verify`
   - Description: Checks the integrity of the project and verifies that the package is valid.
   - Integrity refers to ensuring that the project structure and contents are consistent and have not been corrupted or altered unexpectedly.

6. **Install**
   - Command: `mvn install`
   - Description: Installs the packaged JAR/WAR file into the local Maven repository (e.g., `/home/ec2/vignesh/target/vignesh-1.0-snapshot.jar`) for use by other projects.

7. **Clean**
   - Command: `mvn clean`
   - Description: Cleans up the `target` directory and removes any files created during the build process. This helps to ensure a fresh build environment.

### To run the jar file java -jar **jarName**.jar


# Installing Java and Maven

## Prerequisite: Install Java
```bash
sudo dnf update -y
sudo dnf install java-17-amazon-corretto
```

## Step 1: Install Maven
```bash
wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz
tar -xvzf apache-maven-3.9.4-bin.tar.gz
```
- `-x`: Extract the files.
- `-v`: Verbose output (shows the progress in the terminal).
- `-z`: Uncompress the .gz file.
- `-f`: Specifies the filename to operate on.

```bash
sudo mv apache-maven-3.9.4 /opt/maven
```

## Step 2: Set Up Environment Variables
```bash
vim ~/.bashrc
```

The .bashrc file is a script that runs whenever a new terminal session is started, making it an ideal place to set environment variables.


Add the following lines:
```bash
export M2_HOME=/opt/maven
export PATH=$M2_HOME/bin:$PATH
```
$PATH: This represents the current value of the PATH variable before the modification. By including $PATH at the end of the command, it ensures that the existing directories in PATH are preserved.

Then apply the changes:
```bash
source ~/.bashrc
```
This command reloads the .bashrc file to apply the changes you made. Without this step, the new environment variables wouldn’t be recognized in your current terminal session.

## Step 3: Verify Installation
```bash
mvn -version
```



## Install and Configure Apache Tomcat

### Step 1: Install Java
Tomcat requires Java to run, so ensure Java is installed:
```sh
sudo dnf install java-17-amazon-corretto
```

### Step 2: Download and Extract Tomcat
Download the Tomcat tar file from the official site. Extract it and move it to the `/opt` directory:
```sh
wget https://downloads.apache.org/tomcat/tomcat-10/v10.1.30/bin/apache-tomcat-10.1.30.tar.gz
tar -xvzf apache-tomcat-10.1.30.tar.gz
sudo mv apache-tomcat-10.1.30 /opt/tomcat
```

### Step 3: Start Tomcat
Run the startup script from the `bin` folder:
```bash
cd /opt/tomcat/bin
sh startup.sh
```

Alternatively, create a symbolic link for easier startup:
```sh
sudo ln -s /opt/tomcat/bin/startup.sh /usr/bin/tomcatstart
sh tomcatstart
```

### Step 4: Change the Tomcat Port (Optional)
To modify the default port (8080), edit the `server.xml` file:
```sh
sudo vim /opt/tomcat/conf/server.xml
```
Locate the `<Connector port="8080" ...>` tag and change the port number as needed.

### Step 5: Enable Access to the Manager Application
Edit the `context.xml` file to allow external access:
```sh
sudo find / -name context.xml
sudo vim /opt/apache-tomcat-10.1.30/webapps/host-manager/META-INF/context.xml
sudo vim /opt/apache-tomcat-10.1.30/webapps/manager/META-INF/context.xml
```
Remove the following line from both files:
```xml
<Valve className="org.apache.catalina.valves.RemoteAddrValve" ... />
```

### Step 6: Add User Credentials
Edit `tomcat-users.xml` to add a user with access permissions:
```xml
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>
<role rolename="admin-gui"/>
<user username="admin" password="admin" roles="admin-gui,manager-gui,manager-script,manager-status"/>
```
