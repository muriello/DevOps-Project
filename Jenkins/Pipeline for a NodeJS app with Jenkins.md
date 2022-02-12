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
Before we write any CI/CD pipeline we need an application to test and deploy. In [Getting_Started_With_Git]https://github.com/ValaxyTechDevops/DevOps-Project/tree/master/Git%20and%20Github), We built a [simple node.js application](https://github.com/ValaxyTechDevops/devop-nodejs-app) that responds with “hello world” and hosted it on AWS ec2 instance. In addition, we are going to configure an automation server and host Jenkins on a separate ec2 instance. Jenkins will help us to automate the CI/CD process. On every code change from our Node app repository Jenkins will get notified and it will pull the changes into our Jenkins Server **(step 1)**, install dependencies **(step 2)** and run the integration test **(step 3)**. If all tests pass, Jenkins is going to deploy the app to the node server **(step 4)**. If it fails, a developer will be notified.

<img src="/images/jenkins_workflow.png">

Looking at figure above in step 3, we would like Jenkins to execute our integration test after it builds. To achieve that we need to create a shell script in our project that trigger our test.

Make new /**script**/ folder and a file name **test** without the file extension. Copy and paste the following code into the **test** file:

```sh
#!/bin/sh
npm test
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

***Creating Jenkins [Amazon EC2 instance](https://github.com/ValaxyTechDevops/DevOps-Project/blob/master/AWS/Guide%20to%20create%20Amazon%20EC2%20Instances.md)*** 

Let’s start by creating a second Amazon EC2 instance that will serve our Jenkins app. Follow  ***[this link](https://github.com/ValaxyTechDevops/DevOps-Project/blob/master/AWS/Guide%20to%20create%20Amazon%20EC2%20Instances.md)*** and choose ***“jenkins-app”*** as your hostname.

***Create New User***

Login to the jenkins instance and switch to root user, create a new user, give it sudo privileges and switch to the newly added user:

- Switch to root
```sh
sudo -i
```
- Create a new user
```sh
adduser <username> #input the details in the cmd accordingly
```
- Give it sudo privileges
```sh
usermod -a -G sudo <username>
```
Also, the sudoers file can define privileges such as the commands that can be executed with or without sudo, or if you should be prompted with a password. By default, you should not modify the sudoers file by yourself (the same logic applies to cron jobs for example).

If you were to corrupt this file, you might would not be able to get sudo rights again.

Instead, you are going to use “visudo” : a tool designed to make sure that you don’t make any mistakes
```sh
sudo visudo
```
```At the end of the file, add a new line for the user.```
```sh
username       ALL=(ALL:ALL) ALL
```
By saving and exiting the file, the ```username``` will be automatically added to the sudo group.

By default, the account password will be asked every five minutes in order to perform sudo operations.

If you want to remove the password verification, you can simply add the “NOPASSWD” option.
```sh
username       ALL=(ALL:ALL) NOPASSWD:ALL
```
- switch to the newly added user:
```sh
su - username
```

Jenkins will need to be able to pull changes from the node app repo, therefore, we need to install git on the instance:

```sh
sudo apt-get install git
```

***Install Jenkins***
When faced with repetitive technical tasks, finding automation solutions that work can be a chore. With Jenkins, an open-source automation server, you can efficiently manage tasks from building to deploying software. Jenkins is Java-based, installed from Ubuntu packages or by downloading and running its web application archive (WAR) file — a collection of files that make up a complete web application to run on a server.

- Prerequisites
To follow this tutorial, you will need:
Oracle JDK installed, following our guidelines on [How to Install Java on Ubuntu 20.04](https://github.com/ValaxyTechDevops/DevOps-Project/blob/master/Jenkins/How%20To%20Install%20Java%20with%20Apt%20on%20Ubuntu%2020.04.md).

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
sudo systemctl status jenkins
```

- Opening the Firewall
Jenkins run on port 8080. Remember the Firewall? Lets open the port:

To set up a UFW firewall, Creating Jenkins [Amazon EC2 instance](https://github.com/ValaxyTechDevops/DevOps-Project/blob/master/AWS/Guide%20to%20create%20Amazon%20EC2%20Instances.md)***  Step 15- comfigure security group. By default, Jenkins runs on port 8080.

 Next, We’ll open that port using ufw:

```sh
sudo ufw allow 8080
```

***Note:*** If the firewall is inactive, the following commands will allow OpenSSH and enable the firewall:

```sh
sudo ufw allow OpenSSH
sudo ufw enable
```

Check ufw’s status to confirm the new rules:

```sh
sudo ufw status
```
You’ll notice that traffic is allowed to port 8080 from anywhere

With Jenkins installed and our firewall configured, we can complete the installation stage and dive into Jenkins setup.


To set up your installation, visit Jenkins on its default port, 8080, using your server domain name or IP address: 

```sh
http://your_server_ip_or_domain:8080
```

***Configure Jenkins***
When you navigate to Jenkins homepage you probably noticed additional step you need to do. You need to unlock Jenkins

Copy the Jenkins password hosted on your Jenkins server

```sh
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
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
chmod +x script/*
./script/test
```

In this build step we are going to install dependencies and then run our test shell script.

### Add Git Webhook

We are going to add Git Webhook to inform Jenkins every time a developer push new code to master branch.

Go to node-app GitHub, click on the Settings tab, select Webhooks from the left menu and click on the Add Webhooks button. Enter your Jenkins webhook URL under Payload URL:

```
http://JENKINS.SERVER.IP:8080/github-webhook/

```

and select **Just the Push Event** option. Click the Add webhook button.
<img src="/images/add_webhook.png">


Let’s test what we have so far. Go to your node-app project on your machine and change the version in the package.json to 0.0.2. Commit and push this change to GitHub. After you push, go to your Jenkins job on the browser and observe that the Jenkins job started and completed successfully.


### Deployment

The last piece of the puzzle is deploying our node application into the node-app server when our test passes.

***SSH Authentication***

In order to do that, Jenkins Server will need to ssh into the node-app server, clone the repo, install dependencies and restart the server. Lets set up ssh access between Jenkins server and nodeapp server first.

- On the jenkins Server Switch to Jenkins user:

```sh
su — jenkinsuser
```
- Check if the SSH key for the client machine already exists. This will prevent overwriting the current configuration. You can use the below command to find out
```sh
ls -al ~/.ssh/id_*.pub
```

If you find an existing key, then you can either skip the SSH key generation steps, override the current setup, or create a backup of the existing key. If the key doesn’t exist, you’ll see the following output:

``` ls: cannot access /users/appsadm/.ssh/id_*.pub: No such file or directory ```

- Next, we can proceed to generate the SSH key.

```sh
ssh-keygen -t rsa 
```

- And save the generated private key in */var/lib/jenkins/.ssh/id_rsa*

```sh
sudo mkdir -p /var/lib/jenkins/.ssh/id_rsa

sudo cp -r ~/.ssh/id_rsa /var/lib/jenkins/.ssh/id_rsa
```

- Print the SSH key you just created:

```sh
cat ~/.ssh/id_rsa.pub
```
- This would display an output containing the key starting with ssh-rsa. Copy it! 

- You’ll need to manually add the contents of the id_rsa.pub file to the remote server’s ~/.ssh/authorized_keys file. On the source method you can display the contents of the id_rsa.pub file by using the vi editor or cat command:  login and create the .ssh file if it does not exist.
```sh
mkdir -p ~/.ssh
```
***SSH_public_key*** would be the public key that you copied from the source machine. It will start with ssh-rsa.

- Open the the file where authorized keys are stored:

```sh
vim ~/.ssh/authorized_keys
```
And paste the Jenkins public key we just created into that file. Save by pressing the esc button on your keyboard, type **:x** and press **enter.**

Once the key is copied, you can provide the required permissions to the remote servers .ssh directory by using chmod command.

```sh
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*
```

- With this, we should have successfully activated passwordless SSH, and performed the basic configuration. To test the feature, you can try accessing the remote server via the source server. The command syntax would look like this:

```sh
ssh remote_username@remote_IP_Address
```
If everything worked successfully, then you will be able to login automatically without having to enter the password.

*Success!*

***Automatic Deployment***

We are going to create another shell script that responsible for the deployment. Create a file under *script* called *deploy* and add the following script:

```sh
#!/bin/sh
ssh -i ~/.ssh/key.pem -o StrictHostKeyChecking=no <username>@NODE.SERVER.IP <<EOF
 cd ~/node-app
 git pull
 npm install — production
 pm2 restart all
 exit
EOF
```

This script will SSH to the node server, pull changes from GitHub, install dependencies and restart service.
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
