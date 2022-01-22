## How to set up CI/CD Pipeline for a node.js app with Jenkins

### What is CI/CD?
Continuous Integration and Continuous Deployment are two modern software development practices. Continuous Integration (CI) is the process of automating the build and testing of code every time a team member commits changes to version control. Continuous Deployment (CD) can be thought of as an extension of continuous integration, and is the process of automatically deploying an application after CI is successful. The goal is to minimize lead time; the time elapsed between development writing one new line of code and this new code being used by live users in production.

### Why CI/CD?
There many benefits for CI/CD practices. I am not going to talk about each benefit in detail but I would like to highlight few of them here:

***Continuous Integration Benefits:***
- Fewer bugs
- Less context switching as developers are alerted as soon as they break the build
- Testing costs are reduced
- Your QA team spend less time testing

***Continuous Deployment Benefits:***
- Releases are less risky
- Easy release
- Customers see a continuous stream of improvements
- Expedite development as there’s no need to pause development for releases

### What are we going to build ?
Before we write any CI/CD pipeline we need an application to test and deploy. In [Getting_Started_With_Git](https://github.com/ValaxyTechDevops/DevOps-Project/blob/master/Getting_Started_With_Git/GitHub_Project_For_NodeJS_App.MD), We built a simple node.js application that responds with “hello world” and hosted it on AWS ec2 instance. In addition, we are going to configure an automation server and host Jenkins on a separate ec2 instance. Jenkins will help us to automate the CI/CD process. On every code change from our Node app repository Jenkins will get notified and it will pull the changes into our Jenkins Server **(step 1)**, install dependencies **(step 2)** and run the integration test **(step 3)**. If all tests pass, Jenkins is going to deploy the app to the node server **(step 4)**. If it fails, a developer will be notified.

<img src="/images/jenkins_workflow.png">

Looking at figure above in step 3, we would like Jenkins to execute our integration test after it builds. To achieve that we need to create a shell script in our project that trigger our test.

Make new /**script**/ folder and a file name **test** without the file extension. Copy and paste the following code into the **test** file:

```sh
#!/bin/sh
./node_modules/.bin/mocha ./test/test.js
```

Grant executable permissions:

```sh
chmod +x script/test
```

And test it by executing the shell script from the project root:

```sh
./script/test
```

Boom! integration test is ready and now we ready to push our code to GitHub:

```sh
git add .
git commit -m "simple node app with test"
git push origin master
```

### Set Up Jenkins Server

***Creating Jenkins [Amazon EC2 instance](https://github.com/ValaxyTechDevops/DevOps-Project/blob/master/AWS/Guide_to_create_Amazon_EC2_Instances.md)*** 

Let’s start by creating a second Amazon EC2 instance that will serve our Jenkins app. Follow  ***[this link](https://github.com/ValaxyTechDevops/DevOps-Project/blob/master/AWS/Guide_to_create_Amazon_EC2_Instances.md)*** and choose ***“jenkins-app”*** as your hostname.

***Create New User***

SSH into the new droplet as root user, create a new user, give it sudo privileges and switch to the newly added user:

```sh
ssh root@JENKINS.SERVER.IP
useradd <username> #input the details in the cmd accordingly
usermod -a -G sudo <username>
su — <username>
```

Jenkins will need to be able to pull changes from the node app repo, therefore, we need to install git on the instance:

```sh
sudo apt-get install git
```

***Install Jenkins***
When faced with repetitive technical tasks, finding automation solutions that work can be a chore. With Jenkins, an open-source automation server, you can efficiently manage tasks from building to deploying software. Jenkins is Java-based, installed from Ubuntu packages or by downloading and running its web application archive (WAR) file — a collection of files that make up a complete web application to run on a server.

- Prerequisites
To follow this tutorial, you will need:
Oracle JDK 11 installed, following our guidelines on installing specific versions of OpenJDK on Ubuntu 20.04.

- Get Jenkins:

First, add the repository key to the system:

```sh
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
```
After the key is added the system will return with ``` OK```.

Next, let’s append the Debian package repository address to the server’s ```sources.list```:
```sh
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```
After both commands have been entered, we’ll run update so that apt will use the new repository.
```sh
sudo apt update
```

- Install Jenkins: 

Finally, we’ll install Jenkins and its dependencies.

```sh
sudo apt install jenkins
```

- Start Jenkins: 

```sh
sudo systemctl start jenkins
```

Since systemctl doesn’t display status output, we’ll use the status command to verify that Jenkins started successfully

```sh
sudo systemctl status jenkin
```

Jenkins run on port 8080. Remember the Firewall? Lets open the port:

And now we can access Jenkins on the browser by navigating to:

```sh
http://JENKINS.SERVER.IP:8080
```

***Configure Jenkins***
When you navigate to Jenkins homepage you probably noticed additional step you need to do. You need to unlock Jenkins

Copy the Jenkins password hosted on your Jenkins server

```sh
sudo vim /var/lib/jenkins/secrets/initialAdminPassword
```

Paste the the password into the text field. You are ready to set up Jenkins. First, we would like to add GitHub plugin. From the left menu select **manage Jenkins** and go to **manage plugins.** On the plugins page select the available tab and look for **GitHub plugin**, select its checkbox and click the **Download now and install after restart** button.

Once the installation is complete scroll down the page and select the **Restart Jenkins when installation is complete.** This will restart Jenkins and complete the installation.

***Change Your Jenkins Admin Password***

I suggest at this point to change your Jenkins admin user password. Select **Manage Jenkins** from the left menu and click on **Manage Users**. Select the admin user and choose a new password. You will use the new password when you log in to Jenkins in the future.

***Create Jenkins Job***

We are going to create our Jenkins job that will be responsible for pulling code changes from node-app git repo, install dependencies, run integration test and deploy the application every time a developer push changes to the nodejs-app repo master branch.

Click on New Item button, name the item **node-app** and select **Build a free-style software project** option and click the OK button.

***Configure Jenkins Job***

- Source Code Management: Select git radio button and enter github https link to the node-app repo:

```sh
https://github.com/<username>/node-app.git
```

- Build Triggers: Select option GitHub hook trigger for GITScm polling. This will start our Jenkins job on every git push on the master branch

- Add Build Step: Click on Add Build Step button and select Execute Shell option. Enter the following commands into the text area:

```sh
npm install
./script/test
```

In this build step we are going to install dependencies and then run our test shell script.

### Add Git Webhook

We are going to add Git Webhook to inform Jenkins every time a developer push new code to master branch.

Go to node-app GitHub, click on the Settings tab, select Webhooks from the left menu and click on the Add Webhooks button. Enter your Jenkins webhook URL under Payload URL:

```
*http://JENKINS.SERVER.IP:8080/github-webhook/*

```

and select **Just the Push Event** option. Click the Add webhook button.
<img src="/images/add_webhook.png">


Let’s test what we have so far. Go to your node-app project on your machine and change the version in the package.json to 0.0.2. Commit and push this change to GitHub. After you push, go to your Jenkins job on the browser and observe that the Jenkins job started and completed successfully.

#### Deployment

The last piece of the puzzle is deploying our node application into the node-app server when our test passes.

***SSH Authentication***

In order to do that, Jenkins Server will need to ssh into the node-app server, clone the repo, install dependencies and restart the server. Lets set up ssh access to Jenkins first.

- When we install Jenkins it automatically created Jenkins user. SSH into our Jenkins server as root user:

```sh
ssh root@JENKINS.SERVER.IP
``` 

- Switch to Jenkins user:

```sh
su — jenkins
```

- Generate SSH key:

```sh
ssh-keygen -t rsa
```

And save the generated key in */var/lib/jenkins/.ssh/id_rsa*

- Print the SSH key you just created:

```sh
cat ~/.ssh/id_rsa.pub
```

And Copy the output to your clipboard. Now we are ready to put the public key on the nodejs-app server to complete the authentication between Jenkins server and nodejs-app server.


- SSH into the nodejs-app server as a root and switch to your user:

```sh
ssh root@NODE.SERVER.IP
su - <username>
```

- Open the the file where authorized keys are stored:

```sh
vim ~/.ssh/authorized_keys
```

And copy paste the Jenkins public key we just created into that file. Save by pressing the esc button on your keyboard, type **:x** and press **enter.**


- Set the correct permission on the .ssh folder:

```sh
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*
```

Before we are moving on lets test our SSH set up. If set up is correct we will be able to *SSH from JENKINS.SERVER.IP* as jenkins user to *<username>@NODE.SERVER.IP* without entering a password

```sh
ssh root@JENKINS.SERVER.IP
su - jenkins
ssh <username>@NODE.SERVER.IP
```

*Success!*

***Automatic Deployment***

We are going to create another shell script that responsible for the deployment. Create a file under *script* called *deploy* and add the following script:

```sh
#!/bin/sh
ssh <username>@NODE.SERVER.IP <<EOF
 cd ~/node-app
 git pull
 npm install — production
 pm2 restart all
 exit
EOF
```

This script will SSH to the node server, pull changes from GitHub, install dependencies and restart serve.
Make our new script file executable:

```sh
chmod +x script/deploy
```

Before we commit our changes lets add the deployment step to our Jenkins Job:
<img src="/images/jenkins_execute_shell.png">

And save.

***The Moment of Truth***

We are ready to test everything we built. Go to your nodejs-app project and edit the index.js file to respond with “‘hey world’”. Don’t forget to change your test/test.js to test for that string as well.

Commit and push:

```sh
git add .
git commit -m "add deployment script"
git push origin master
```

After you push you should see Jenkins Job starts. When complete, you should see your changes on *http://NODE.SERVER.IP:7000*

And we are DONE!
