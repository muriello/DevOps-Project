#  Install & configure Maven build tool on Jenkins
Maven is a code build tool which used to convert your code to an artifact. this is a widely used plugin to build in continuous integration


#### Prerequisites
1. [Jenkins server](https://github.com/ValaxyTechDevops/DevOps-Project/blob/master/LAB_1/Jenkins_Installation.MD)
2. The Gremlin libraries require Java 8. install java 8 if you dont have it installed yet. 
   - Enter the following to set Java 8 as the default runtime on your EC2 instance.
    ```sh
    sudo /usr/sbin/alternatives --config java

    #When prompted, enter the number for Java 8.
    ```
  - When prompted, enter the number for Java 8.
  - Enter the following to set Java 8 as the default compiler on your EC2 instance.
    ```sh
    sudo /usr/sbin/alternatives --config javac

    #When prompted, enter the number for Java 8.
    ```
#### [Install Maven on Jenkins](https://linuxize.com/post/how-to-install-apache-maven-on-centos-8/)
1. Download maven packages https://maven.apache.org/download.cgi onto Jenkins server. In this case, I am using /opt/maven as my installation directory

 - Link :  https://dlcdn.apache.org/maven/maven-3/3.8.4/binaries/apache-maven-3.8.4-bin.tar.gz
 
    ```sh
     # change directory to /opt
     cd /opt
    ```
    ```sh
     # downloading maven version 3.8.4
     wget https://dlcdn.apache.org/maven/maven-3/3.8.4/binaries/apache-maven-3.8.4-bin.tar.gz
     tar -xvzf apache-maven-3.8.4-bin.tar.gz
    ```
    ```sh
     # unzip the file 
     tar -xvzf apache-maven-3.8.4-bin.tar.gz
    ```

    ```sh
     # move the unzipped folder to maven 
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

