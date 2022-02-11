#  Install & configure Maven build tool on Jenkins
Maven is a code build tool which used to convert your code to an artifact. this is a widely used plugin to build in continuous integration


#### Prerequisites

1. [Jenkins server](https://github.com/ValaxyTechDevops/DevOps-Project/blob/master/LAB_1/Jenkins_Installation.MD)


#### [Install Maven on Jenkins](https://linuxize.com/post/how-to-install-apache-maven-on-centos-8/)
1. Download tomcat packages from  https://maven.apache.org/download.cgi onto /opt on EC2 instance
   > Note: Make sure you change `<version>` with the maven version which you download. 
   > I am downloading maven version 3.8.4
   ```sh 
   # Create tomcat directory
   cd /opt
   wget https://dlcdn.apache.org/maven/maven-3/3.8.4/binaries/apache-maven-3.8.4-bin.tar.gz
   tar -xvzf apache-maven-3.8.4-bin.tar.gz
   ```

    ```sh
     # Rename the apache-maven-`<version>` folder to maven 
     mv apache-maven-3.8.4 maven
    ```
	
1. Setup M2_HOME and M2 paths in .bash_profile of the user and add these to the path variable
   find JAVA_HOME path and also incode in the .bash_profile  ```find / -name java-11*```
    
   ```sh
   vi ~/.bash_profile
   M2_HOME=/opt/maven
   M2=/opt/maven/bin
   JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.13.0.8-1.amzn2.0.3.x86_64
   PATH=<Existing_PATH>:$JAVA_HOME:$M2_HOME:$M2
   ```
#### Checkpoint 
1. type the following command to reload the .bash_profile

    ```sh
    source .bash_profile
    ```
 Now check for maven version 
    ```sh
    mvn --version
    ```
So far we have completed the installation of maven software to support maven plugin on the jenkins console. Now jump onto your Jenkins to complete the remaining steps. 

### Setup maven on Jenkins console
1. Install maven plugin without restart  
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Invoker`
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Integration`

2. Configure maven path
  - `Manage Jenkins` > `Global Tool Configuration` > `Maven`

