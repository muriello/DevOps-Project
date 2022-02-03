# DevOps-Project
guide to set up a ci/cd workflow 
https://www.ktexperts.com/how-to-install-jenkins-in-amazon-linux-machine/


- Launch Jenkins Machine. 
- Connect to Jenkins Linux EC2 Terminal through MobaXterm.
- Switch to root user.  
```
sudo su
```
- Update Server Packages.
```
yum update -y
```
- Install the Java. 
```
yum -y install java-1.8.0-openjdk
java -version
```
- Configure the Java Home Location.
```
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.191.b12-1.el7_6.x86_64
```
- Download and Install Jenkins. 
```
yum -y install wget
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum -y install jenkins
```
- Start the Jenkins.
```
systemctl start jenkins
chkconfig jenkins on
```
- Access the Jenkins. 
```
Copy the Public IP of Jenkins
```