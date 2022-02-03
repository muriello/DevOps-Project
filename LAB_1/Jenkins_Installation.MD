# Install Jenkins on Amazon Linux Machine
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.

### Prerequisites
- EC2 Instance 
   - With Internet Access
   - Security Group with Port `8080` open for internet and open SSH/HTTP 
- Java 8 and 11 should be installed  

### Steps to Follow
- Launch Jenkins Machine.
- Connect to Jenkins Linux EC2 Terminal through Mobaxterm.
- Switch to root user.
- Update Server Packages.
- Install the Java.
- Configure the Java Home Location.
- Download and Install Jenkins.
- Start the Jenkins.
- Access the Jenkins

#### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
- Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- `Skip` Plugin Installation; _We can do it later_
- Change admin password
   - `Admin` > `Configure` > `Password`
- Configure `java` path
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK`  
- Create another admin user id

### Test Jenkins Jobs
- Create “new item”
- Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
- Under the Build section
	Execute shell: echo "Welcome to Jenkins Demo"
- Save your job 
- Build job
- Check "console output"
