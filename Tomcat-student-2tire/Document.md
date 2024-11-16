Documentation for Tomcat & MariaDB Setup with AWS RDS Integration
This document provides a step-by-step guide to set up Apache Tomcat 9.0.97, MariaDB 10.5, and integrate it with an AWS RDS database for a student management application.

RDS Documentation - Step-by-Step Notes:
Create an RDS Database:

Log into AWS and create an RDS instance. Choose the database engine (e.g., MySQL, MariaDB) and configure settings as needed.
Create an Instance:

Set up a new EC2 instance where the Apache Tomcat server will be installed and configured.
Attach Security Group Rules:

Attach inbound rules for the following ports in the security group:
Port 3306 (MySQL/MariaDB)
Port 8080 (Apache Tomcat)
Connect to the Instance:

SSH into the EC2 instance. Run the following command to log in as root user:
bash
Copy code
sudo -i
Download Apache Tomcat:

Use curl to download Apache Tomcat:
bash
Copy code
curl -O https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.91/bin/apache-tomcat-9.0.91.tar.gz
Check the downloaded file:
bash
Copy code
LS
Unzip the file and move it to the /opt/ directory:
bash
Copy code
mv apache-tomcat-9.0.91 /opt/
Login via MobaXterm:

Use MobaXterm to connect via SSH. Input the public IP and PEM key:
Public IP: 13.201.85.99
PEM Key: GAURAV.PEM
Connect and Check Apache File Location:

Run the following commands:
bash
Copy code
sudo -i
cd /opt
ls
Upload Connector JAR and WAR Files:

Upload mysql-connector.jar and student.war files to your EC2 instance.
Move them into the correct directories:
bash
Copy code
mv mysql-connector.jar /opt/apache-tomcat-9.0.91/lib/
mv student.war /opt/apache-tomcat-9.0.91/webapps/
Check Tomcat Files and Install Java:

Navigate to the Tomcat directory and verify the files:
bash
Copy code
cd /opt/apache-tomcat-9.0.91/
ls
cd bin/
ls
cd conf/
ls
Search for the latest version of Java and install it:
bash
Copy code
yum search java
yum install amazon-corretto-11 -y
Create context.xml File:

Use the vim editor to create and configure the context.xml file:
bash
Copy code
vim context.xml
Add necessary configuration details, then exit insert mode using :wq.
Start Tomcat Server:

Start the Apache Tomcat server using the following command:
bash
Copy code
bash catalina.sh start
Install MariaDB:

Search for MariaDB and install it:
bash
Copy code
yum search maria
yum install mariadb105.x86_64 -y
Connect to the RDS Database:

Use the following command to connect to your RDS instance:
bash
Copy code
mysql -h database-1.c3yqqwyeoe4f.ap-south-1.rds.amazonaws.com -u admin -pAdmin123
Check Databases:

Check if the database is accessible:
bash
Copy code
SHOW DATABASES;
Create studentapp Database:

Create the studentapp database and a table for storing student information:
bash
Copy code
create database studentapp;
use studentapp;
CREATE TABLE if not exists students (
  student_id INT NOT NULL AUTO_INCREMENT,
  student_age VARCHAR(3) NOT NULL,
  student_name VARCHAR(100) NOT NULL,
  student_addr VARCHAR(100) NOT NULL,
  student_qual VARCHAR(20) NOT NULL,
  student_percent VARCHAR(10) NOT NULL,
  student_year_passed VARCHAR(10) NOT NULL,
  PRIMARY KEY (student_id)
);
Access the Web Application:

In your browser, access the application using the EC2 instance's public IP:
bash
Copy code
http://13.201.85.99:8080/student/





