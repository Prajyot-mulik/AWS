Documentation for Tomcat & MariaDB Setup with AWS RDS Integration
This document provides a step-by-step guide to set up Apache Tomcat 9.0.97, MariaDB 10.5, and integrate it with an AWS RDS database for a student management application.


Prerequisites:
AWS EC2 Instance running a compatible Linux distribution (Amazon Linux, Ubuntu, etc.).
RDS Instance running MariaDB 10.5 or later.
SSH Access to your EC2 instance.
MobaXterm or any other SSH client for remote server access.


Step 1: Create RDS Database
Create a MariaDB RDS Instance:
Go to AWS RDS Console and create a MariaDB 10.5 database instance.
Ensure your RDS instance is publicly accessible if you need to access it from outside AWS.
Set up the correct inbound security group rules allowing access to port 3306 (MariaDB) and port 8080 (Tomcat).


Step 2: Install Tomcat on EC2
Download Tomcat: On your EC2 instance, run:

curl -O https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.97/bin/apache-tomcat-9.0.97.tar.gz


step 3 : Extract and Move Tomcat Files: After downloading, extract the file:
tar -xvf apache-tomcat-9.0.97.tar.gz

step 4 :Move the Tomcat directory to /opt/:
mv apache-tomcat-9.0.97 /opt/

step 5 :Verify Tomcat Files: Navigate to the Tomcat directory to verify:
cd /opt/
ls

----------------------------------------------------------------------------------------------------

Step 6 : Upload Connector & Application (WAR File)
Upload mysql-connector.jar and student.war to the EC2 instance. You can do this using MobaXterm or an SCP client to the /home/ec2-user/ directory.

Move the Files to Correct Directories:

mv mysql-connector.jar /opt/apache-tomcat-9.0.97/lib/
mv student.war /opt/apache-tomcat-9.0.97/webapps/

Step 4: Install Java
Install Java 11 Amazon Corretto: Run the following command to install Java 11:

yum install java-11-amazon-corretto -y
Verify Java Installation: Check that Java is installed by running:

----------------------------------------------------------------------------------------------------


Step 5: Configure context.xml
Edit Tomcat's context.xml File: Navigate to the conf/ directory:

cd /opt/apache-tomcat-9.0.97/conf/
Edit context.xml: Open the file with vim:
----------------------------------------------------------------------------------------------------

vim context.xml
Insert Configuration Data: Add the required <Resource> tag for database connectivity in context.xml:

xml
Copy code
<Resource name="jdbc/studentDB"
          auth="Container"
          type="javax.sql.DataSource"
          maxActive="100" maxIdle="20" maxWait="-1"
          username="admin" password="your_password"
          driverClassName="org.mariadb.jdbc.Driver"
          url="jdbc:mariadb://database-1.c3yqqwyeoe4f.ap-south-1.rds.amazonaws.com:3306/studentapp"
          validationQuery="SELECT 1" />

----------------------------------------------------------------------------------------------------

Step 6: Start Tomcat
Start Tomcat: To start Tomcat, run:

bash
Copy code
cd /opt/apache-tomcat-9.0.97/bin/
bash catalina.sh start
Verify Tomcat is Running: You can check the Tomcat logs to verify it's running correctly:

----------------------------------------------------------------------------------------------------