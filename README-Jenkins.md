# Jenkins Setup

C:\Users\ksomalin\cisco-files\Jenkins

java -jar jenkins.war

To download and run the WAR file version of Jenkins:

Download the latest stable Jenkins WAR file to an appropriate directory on your machine.
http://mirrors.jenkins.io/war-stable/latest/jenkins.war

Open up a terminal/command prompt window to the download directory.

Run the command java -jar jenkins.war

java -jar jenkins.war --httpPort=9090

initial password:
C:\Users\ksomalin\.jenkins\secrets\initialAdminPassword

Browse to http://localhost:8080 and wait until the Unlock Jenkins page appears.

Continue on with the Post-installation setup wizard below.

User/pwd: admin/admin


# References: https://www.jenkins.io/doc/book/installing/

$ Issues Faced

1) SSL Handshake Exception during startup of the server:

You have mainly two options to solve the issue:

1) Install an SSL Certificate for connecting to Jenkins a secure service (SSL/TLS).

See the following link to follow this approach:

https://support.cloudbees.com/hc/en-us/articles/203821254-How-to-install-a-new-SSL-certificate-

2) Another quick hack is to simply switch the default update site from https to http. Choose Manage Jenkins->Plugin Manager->Advanced

You will see the following update site default:

Now change it to http://updates.jenkins-ci.org/update-center.json

You are done!
# Reference: http://www.mastertheboss.com/cool-stuff/jenkins/solving-jenkins-sslhandshakeexception