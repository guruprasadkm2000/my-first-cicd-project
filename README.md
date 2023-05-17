# my-first-cicd-project
my-first-cicd-project using jenkins, maven, tomcat and github

Apache Tomcat is an open-source web server and servlet container developed by the Apache Software Foundation. It is designed to provide a robust and efficient environment for running Java-based web applications. Tomcat implements the Java Servlet, JavaServer Pages (JSP), and Java WebSocket technologies, allowing developers to build and deploy dynamic web applications.
Key features of Apache Tomcat include:
1.	Servlet Container: Tomcat serves as a container for Java servlets, managing their lifecycle and handling HTTP requests and responses.
2.	JSP Support: Tomcat supports JavaServer Pages (JSP), a technology that allows the dynamic generation of web content using Java code embedded within HTML pages.
3.	HTTP Web Server: In addition to its servlet and JSP capabilities, Tomcat can also function as a standalone web server, serving static content and handling HTTP requests directly.
4.	Lightweight and Efficient: Tomcat is known for its lightweight nature, consuming minimal system resources and providing fast performance.
5.	Configuration and Management: Tomcat provides a comprehensive set of configuration options to customize its behavior, including security, session management, and connection pooling. It also offers a web-based management interface for monitoring and administering the server.
6.	Widely Adopted: Apache Tomcat is widely used and has a large community of developers and users. It is the reference implementation for the Java Servlet and JavaServer Pages specifications and is supported by numerous tools and frameworks in the Java ecosystem.
Apache Tomcat 
•	➔ Apache Tomcat is a web server 
•	➔ Tomcat is used to run Java Web Applications 
•	➔ Tomcat is free & open-source software 
•	➔ Tomcat developed by Apache Organization 
•	➔ Tomcat developed by using ‘Java’ programming language 
•	➔ Tomcat server runs on 8080 port by default (we can change that port in server.xml file) 

Overall, Apache Tomcat is a popular choice for deploying Java web applications, providing a reliable and scalable platform for running servlets, JSP pages, and other Java-based web components.




How to install on Amazon Linux 2 (EC2 Instance)
(Before installing tomcat, we need to install java jdk)
sudo yum update
yum install java-1.8* or sudo yum install java-1.8.0-openjdk
java -version

Download Apache Tomcat using ‘wget’ command.
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.75/bin/apache-tomcat-9.0.75.tar.gz

Extract Apache Tomcat tar file using tar command.
tar -xvf apache-tomcat-9.0.75.tar.gz

We can see Apache Tomcat files using. 
ls command.
 
To deploy applications into Tomcat server we will use Host Manager 
cd apache-tomcat-9.0.75/webapps/manager/META-INF/context.xml 
or

cd apache-tomcat-9.0.75
vi ./webapps/manager/META-INF/context.xml     
change >>> allow =      ”.*”

We need to add users in tomcat server to perform admin activities.
File Location: <tomcat-folder>/conf/tomact-users.xml
or
cd apache-tomcat-9.0.75
cd conf
vi tomcat-users.xml

Add below Roles and Users in tomcat-users.xml file 
<role rolename="manager-gui" /> 
<role rolename="manager-script" /> 
<role rolename="admin-gui" /> 
<user username="tomcat" password="tomcat" roles="manager-gui" /> 
<user username="admin" password="admin" roles="manager-gui,admin-gui,manager-script"/>

:wq to save and exit. 

After adding users, go to bin directory and start the Tomcat server. 
Start the tomcat server using ‘sh startup.sh’ command 

Make 8080 port number is enabled in your EC2 Instance Security Group 

Click on Host Manager to deploy the application (It will ask credentials, enter the credentials which we have configured in ‘tomcat-users.xml’ file) 
Admin 
P > admin

Go to maven web application and package that application as war file (war file will be created in target folder) 

We can deploy Maven Web Application ‘war’ file into Tomcat Server from Host Manager Console 
➔ Choose War file and click on ‘Deploy’ button 
➔ We can see our application which got deployed in Tomcat Server 

 



Jenkins ---

sudo apt-get update
sudo apt-get install default-jre
java -version

Add Jenkins key to repository
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key |sudo gpg --dearmor -o /usr/share/keyrings/jenkins.gpg
sudo sh -c 'echo deb [signed-by=/usr/share/keyrings/jenkins.gpg] http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

sudo apt update

Install Jenkins software.
sudo apt install Jenkins
sudo systemctl start jenkins
sudo systemctl status Jenkins
Configure Inbound rule to allow 8080 protocols in the instance.

To unlock Jenkins, we need admin password.
sudo cat /var/lib/jenkins/secrets/initialAdminPassword


 
============================
Jenkins Job Creation Process
=============================
1) Login into Jenkins

2) Configure Maven in Global Tool Configuration (Jenkins will download maven)
-> Manage Jenkins
-> Global tools Configurations
-> Add Maven

3) Install 'Deploy To Container' Plugin (To deploy war to tomcat server)
-> Manage Jenkins
-> Manage Plugins
-> Go to available tab
-> search for 'Deploy To Container' plugin
-> Click on install without re-start
Note: Git s/w will be available by default in Jenkins Gloabl Tools Configuration
4) Create Free Style Project 
5) Enter Git Repo URL
6) Build Trigger (configure Maven which is added in Global Tools and Provide Maven Goals as clean package)
7) Add Tomcat Server in Post Build Action For deployment
8) Save the Job configuration
9) Run the Job
10) See Tomcat Dashboard (Application should display) and access the application


 
=================
Poll SCM
=================
-> Click on Job name
-> Click On Configure
-> Configure Poll SCM with cron expression as ( * * * * *)
-> Every minute it will check for code changes, if code changes available then jenkins job will run

 
 
===================
Infrastructure Setup 
====================
-> Create Linux VM using EC2 in AWS cloud and install tomcat server
-> Create Linux VM using EC2 in AWS cloud and install Jenkins server
-> Clone Git Hub Repository


 
===========================
Build & Deployment Process
===========================
Download project from git hub
package the project using maven
Maven will create war file
Deploy war file into tomcat  (post build action)
Note: Above build & deployment process can be automated using Jenkins
Access application using URL in browser
