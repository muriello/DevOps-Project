# What are we going to build?
We are going to build a simple NodeJS app and host it(in this guide, I am going to use AWS ec2 instance. you can use whatever you want). This guide will show you how to config and work with Github to deploy the NodeJS app to your server. 

# Create a NodeJS App
As the first thing in this guide, we are going to create a NodeJS application locally. Here we just create a simple application that responds with the “Hello World” text. First, we will use our Github repo we created  . The below link will show how I did it. You can change the settings as you want such as setting private/public accessibility of your repository.https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-on-github/creating-a-new-repository. 

1. Now let’s clone the repository and navigate to it:
   ```sh
   git clone https://github.com/<username>/node-app.git
   cd node-app 
   ```
2. Then run below commands to create package.json file and add the required dependencies and other content and then run following command to install the dependencies. Here are some descriptions of the packages that we use.
   ```sh
   npm init // fill the required things CLI asked
   npm i express
   npm i mocha supertest --save-dev
   ```
**express**: Node framework

**mocha**: Test framework for NodeJS ( You can choose another testing framework if you wish like Jasmin, Jest, Tape, etc.)

**supertest**: Provide a high-level abstraction for testing HTTP npm install

3. Okay, now everything is ready. let’s create a file called **index.js**** to write the code to create an express server and show the **“Hello World”** text as the response of the **“/”** endpoint.

   ```sh
    // importing express framework
    const express = require("express");

    const app = express();

    // Respond with "hello world" for requests that hit our root "/"
    app.get("/", function (req, res) {
     return res.send("Hello World");
    });

    // listen to port 7000 by default
    app.listen(process.env.PORT || 7000, () => {
      console.log("Server is running");
    });

    module.exports = app;
   ```
4. Now you can run the above code using the below command. Then visit **http://localhost:7000** to see the “Hello World” output.

   ```sh
   node index.js
   ```
# Write Test Cases
  we are going to write the test case to test our **“/”** endpoint response equals to **“Hello World”**. to do that, let’s create a folder called **/test/** and create a file called ***test.js*** inside that. Now let’s write the test code as bellow.

   ```sh
    const request = require("supertest");
    const app = require("../index");

    describe("GET /", () => {
      it("respond with Hello World", (done) => {
        request(app).get("/").expect("Hello World", done);
      })
    });
   ```
  now just run below command to run test cases you wrote to see whether it gets passed or not.
  
   ```sh
   npm test
   ```
Now we can push our changes to Github repository we created. Before that create .gitignore file and add files/folders you need to ignore from the git repository. here is my example **.gitignore** file I used in this guide. Now let’s push the files to Github

   ```sh
   git add .
   git commit -m "node app with test cases"
   git push origin master
   ```

# Deploy NodeJS App on Server
Now we need to deploy our application in our ec2 server. to do that, we are going to :

1. ***Create Our Server (Amazon EC2 instance)*** and login using visual studio code 
   
   https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html

   https://medium.com/@christyjacob4/using-vscode-remotely-on-an-ec2-instance-7822c4032cff
   

2. Create a new user
   ```sh
   ssh root@SERVER.IP
   adduser <lastname>
   usermod -a -G sudo <username>
   su — username
   ```
3. Generate an SSH key pair 
   ```sh
   ssh-keygen -t rsa
   cat ~/.ssh/id_rsa.pub
   ```

4. Run below commands to give that user login access via SSH without a password. then you can easily log in to the server by running ssh @SERVER.IP.
   ```sh
   cd
   mkdir .ssh
   chmod 700 .ssh/
   vim ~/.ssh/authorized_keys # here paste your local ssh key as we did earlier
   chmod 600 ~/.ssh/*
   ```
5. Now install the git in the server. Then pull the source code and run it on the server.
   ```sh
   sudo apt-get install git
   ssh-keygen
   cat ~/.ssh/id_rsa.pub #copy the text
   ```
6. Now we copied our new user ssh key. just paste it as a Github repository deploy key. then clone the repository and run it on the server.
   ```sh
   git clone https://github.com/<username>/node-app.git
   cd node-github-demo
   npm install --production
   sudo ufw allow 7000 # this will open port 7000 to access via web
   sudo npm install pm2 -g
   pm2 start index.js --name node-app
   ```

Now our app is running through PM2 as node-app. just got to **http://SERVER.IP:7000** to check that.