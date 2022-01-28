## Introduction

Java is the most popular object-oriented, robust, platform-independent programming language. There are multiple applications required for your system required Java on your system. This guide will help you to install Java (OpenJDK 11 and OpenJDK 8) stable releases or Oracle Java 14 on your Ubuntu 20.04 LTS (Focal Fossa) system. You will also find the instruction’s to switch between multiple installed Java versions.

###  Installing Java on Ubuntu 20.04

The Java 11 is the latest LTS release available for installation. The default Ubuntu packages repositories contain the packages for the OpenJDK 11. The default repository also contains OpenJDK 8 previous stable release packages.

The JDK packages provide the full Java development libraries, helpful for the development systems. To run any Java application, you just needed a Java runtime environment (JRE).

1. Install Java 11 on Ubuntu
Run the below command to install OpenJDK 11 package on your system.

```sh
sudo apt update
sudo apt install openjdk-11-jdk
```

Once the installation is complete, you can verify it by checking the Java version:

```sh
java -version
```
```
openjdk version "11.0.7" 2020-04-14
OpenJDK Runtime Environment (build 11.0.7+10-post-Ubuntu-3ubuntu1)
OpenJDK 64-Bit Server VM (build 11.0.7+10-post-Ubuntu-3ubuntu1, mixed mode, sharing)
```
OpenJDK 11 has been installed. Continue to install OpenJDK 8
2. Install Java 8 on Ubuntu
Java 8 is the previous stable release, Most of the java-based applications works on it. Run the below command to install OpenJDK 8 package on your system.

```sh
sudo apt update
sudo apt install openjdk-8-jdk
```

Once the installation is complete, you can verify it by checking the Java version:

```sh
java -version
```
```
openjdk version "1.8.0_252"
OpenJDK Runtime Environment (build 1.8.0_252-8u252-b09-1ubuntu1-b09)
OpenJDK 64-Bit Server VM (build 25.252-b09, mixed mode)
```

All done, you have successfully installed Java (OpenJDK) on your Ubuntu system.

### Switch Between Multiple Java Versions

Most of the Unix/Linux-based systems allow to the installation of multiple Java versions on one system. If you also have multiple Java versions installed on your system. You can change to the default java version as per your requirements.

The update-alternatives provides you option to maintain symbolic links for the default commands. To change default Java version run command on terminal:


```sh
update-alternatives --config java
```

This will show the list of all java binaries installed on your system. Enter a number to select the default Java version and press enter.

The above command will change the Default Java version on your system by changing the link references to java binary. Now, again run command **java -version** to view the default Java version.


## Conclusion

In this tutorial, you have learned about the installation of multiple Java on the Ubuntu 20.04 systems. Also found a solution to change the default Java version via the command line.