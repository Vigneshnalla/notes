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
Add the following lines:
```bash
export M2_HOME=/opt/maven
export PATH=$M2_HOME/bin:$PATH
```
Then apply the changes:
```bash
source ~/.bashrc
```
This command reloads the .bashrc file to apply the changes you made. Without this step, the new environment variables wouldn’t be recognized in your current terminal session.

## Step 3: Verify Installation
```bash
mvn -version
```

Ec2:
===
    we need to install java before working with manve
    Install java
sudo dnf update -y
sudo dnf install java-17-amazon-corretto
Install Maven
wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz
tar -xvzf apache-maven-3.9.4-bin.tar.gz
        -x: Extract the files.
        -v: Verbose output (shows the progress in the terminal).
        -z: Uncompress the .gz file.
        -f: Specifies the filename to operate on.
sudo mv apache-maven-3.9.4 /opt/maven
Set Up Environment Variables
vim ~/.bashrc. and add below lines
    The .bashrc file is a script that runs whenever a new terminal session is started, making it an ideal place to set environment variables.
export M2_HOME=/opt/maven
export PATH=$M2_HOME/bin:$PATH
source ~/.bashrc
Explanation: This command reloads the .bashrc file to apply the changes you made. Without this step, the new environment variables wouldn’t be recognized in your current terminal session.

Verify Installation : mvn -version