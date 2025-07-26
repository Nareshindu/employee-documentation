### Backend

# EmployeeDetailsBoot - Java Spring Boot JSP Project

## Project Overview

**EmployeeDetailsBoot** is a Spring Boot-based Java web application designed to manage employee data using a PostgreSQL database and JSP (Java Server Pages) for the front-end view layer. It is packaged as a standalone **JAR** file with an embedded Tomcat server.

---

## Technologies Used

- **Java 1.8**
- **Spring Boot 2.6.3**
- **Maven**
- **PostgreSQL**
- **Spring Data JPA**
- **JSP (Tomcat Jasper)**
- **JSTL**

---

The Backend service is responsible for adding the given values to the database. The Backend service is written in **Java**, so we need to install **Java 8** and **Apache Maven** for compiling and running the application.

---

## Installation steps of Java 8 (OpenJDK 1.8)

By default, Java 8 is available via DNF on RHEL. Install it using:
```
sudo dnf install -y java-1.8.0-openjdk java-1.8.0-openjdk-devel
```

### Verify Java Installation:
```
sudo java -version
```
---

### Set JAVA_HOME Environment Variables:

Add the below environment variables to `~/.bashrc` or `/etc/profile.d/java.sh`:
```
sudo vi ~/.bash_profile
```
# or
```
sudo vi /etc/profile.d/java.sh
```
---
Add the ENVs to above any one file:
```
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
export PATH=$JAVA_HOME/bin:$PATH
```
---

## Reload after adding ENVs

```
source ~/.bashrc 
```
# or
```
source /etc/profile.d/java.sh
```
In which file you have added ENVS reload only that file

---

## Maven Installation

To build and run this project, you need to have **Apache Maven** installed on your system.

---

### Prerequisites

- **Java 1.8 or above**  
- **Maven 3.6+** recommended

---

##  Step-by-Step Installation

### Maven Installation on RHEL (Red Hat Enterprise Linux)

To build and run this project, Apache Maven must be installed on your RHEL system.

---

## Step-by-Step Installation (Manual)

## To download the latest version of Apache Maven checkout the below website (Optional)
```
https://maven.apache.org/download.cgi
```
## Change the directory
```
sudo cd /opt
```
## Download the mvn zip file from internet using below command
```
sudo wget https://archive.apache.org/dist/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
```
---

### Extract the Archive
```
sudo tar -xvzf apache-maven-3.9.6-bin.tar.gz
```
## Rename the maven folder
```
sudo mv apache-maven-3.9.6 apache-maven
```
---

## Set Up Environment Variables

Create a new profile file for Maven:
```
sudo vi /etc/profile.d/maven.sh
```
## Paste the ENVS inside the file /etc/profile.d/maven.sh
```
export M2_HOME=/opt/apache-maven
export PATH=${M2_HOME}/bin:${PATH}
```
## After applying reload the changes 
```
source /etc/profile.d/maven.sh
```
---

## Verify Maven Installation
```
mvn -version
```

## Expected output:

Apache Maven x.x.x (bc0240f3c744dd6b6ec2920b3cd08dcc295161ae)
Maven home: /opt/apache-maven
Java version: 1.8.0_462, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.462.b08-3.el9.x86_64/jre
Default locale: en, platform encoding: UTF-8
OS name: "linux", version: "5.14.0-362.18.1.el9_3.x86_64", arch: "amd64", family: "unix"

---

## directly we can install maven like java via YUM (Otional and second way to install maven)
```
sudo yum install maven
```
---

<!-- ## Create one Directory for project
```
sudo mkdir /employee
```
## change the directory
```
cd /employee
```
## Create employee user
```
sudo useradd employee
``` -->

## Download the application code to /opt directory.
```
sudo git clone https://github.com/Nareshindu/employee-code.git
```
## Do changes in application.properties change the database ip-address 

```
sudo vi /opt/employee-code/Employee-project/src/main/resources/application.properties
```
## change the Ip-address and if you want you can change username, password and database name also

spring.datasource.url=jdbc:postgresql://172.31.34.229:5432/ptdb                                                     
spring.datasource.username=employee                                                                                 
spring.datasource.password=Admin@123 

## Change the permission for project 
```
chown -R ec2-user:ec2-user /opt/employee-code
```
## To delete old target directory and build the package after deleting again
```
mvn clean install
```
## We need to setup a new service in systemd so systemctl can manage this service

Setup SystemD emplyee Backend Service
```
sudo vi /etc/systemd/system/backend.service
```
```
[Unit]
Description=Java Backend Service
After=network.target

[Service]
User=ec2-user
Group=ec2-user
WorkingDirectory=/opt/employee-code/Employee-project
ExecStart=/usr/bin/java -jar /opt/employee-code/Employee-project/target/EmployeeDetailsBoot.jar
SuccessExitStatus=143
Restart=always
RestartSec=5
SyslogIdentifier=java-backend

[Install]
WantedBy=multi-user.target
```
## Load the service.
```
sudo systemctl daemon-reload
```
## Enable the service.
```
sudo systemctl enable backend
```
## Start the service
```
sudo systemctl start backend
```
## Access this employee web-application on browser
```
http://18.234.229.147:8080/
```
## Verification from backend server

We can check data by using client package called PostgreSQL client or psql client.

---

### Add PostgreSQL YUM Repository  
This enables access to PostgreSQL versions from the official PostgreSQL repo.
```
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

---

## Disable the default PostgreSQL module  
Prevents RHEL from installing the default older PostgreSQL version.
```
sudo dnf -qy module disable postgresql
```
---

### Install postgresql13 Client Only  
Installs the psql command-line client without installing the database server.
```
sudo dnf install -y postgresql13
```
---

### Verify the client Installation
```
psql --version
```
You should see something like:

`psql (PostgreSQL) 13.x`

---

### To test the remote connection from backend server
```
psql -h ip-addr -U username -d dbname
```
---
