# What are we going to build?
We are going to build a simple NodeJS app and host it(in this guide, I am going to use AWS ec2 instance. you can use whatever you want). This guide will show you how to config and work with Github to deploy the NodeJS app to your server. 

# Create a NodeJS App
As the first thing in this guide, we are going to create a NodeJS application locally. Here we just create a simple application that responds with the “Hello World” text. First, we will use our Github repo we created. This [link](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-on-github/creating-a-new-repository) will show how I did it. You can change the settings as you want such as setting private/public accessibility of your repository.

1. Now let’s clone the repository and navigate to it:
   ```sh
   git clone https://github.com/<username>/node-app.git
   cd node-app 
   ```
2. The first step when building a node app is creating package.json file. In this file, we list the application dependencies. Run below commands to create package.json file and add the required dependencies and other content and then run following command to install the dependencies. Here are some descriptions of the packages that we use.
   ```sh
   npm init # fill the required things CLI asked accordingly 
   npm i express
   npm i mocha supertest --save-dev
   ```
  - **express**: Node framework

  - **mocha**: Test framework for NodeJS ( You can choose another testing framework if you wish like Jasmin, Jest, Tape, etc.)

  - **supertest**: Provide a high-level abstraction for testing HTTP npm install

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

1. ***Create Our Server [Amazon EC2 instance](https://github.com/ValaxyTechDevops/DevOps-Project/blob/master/AWS/Guide_to_create_Amazon_EC2_Instances.md)*** and login using Mobaxterm 

2. Open your terminal on your local machine and login into your nodejs-app server as a root user:
   ```sh
   ssh root@SERVER.IP

   ```
3. Now let’s create a new user to do the server configuration work and name it after your last name:
   ```sh
   adduser <lastname>

   ```
4. Choose user password and follow the prompts. Before we are switching to our new user we need to give him sudo privileges:
   ```sh
   usermod -a -G sudo appuser

   ```
5. Now you can switch to your new user.
   ```sh
   su — username

   ```  
6. Our ec2 server does not come with Node and Git. Lets install node and git using app-get:
   
   - Installing Node.js with Apt from the Default Repositories. To get this version, you can use the apt package manager. Refresh your local package index first by typing 
   ```sh
   sudo apt update

   ```  
   Then install Node.js:
   ```sh
   sudo apt install nodejs

   ```  
   Check that the install was successful by querying node for its version number:
   ```sh
   node -v
   ``` 
   If the package in the repositories suits your needs, this is all you need to do to get set up with Node.js. In most cases, you’ll also want to also install npm, the Node.js package manager. You can do this by installing the npm package with apt:
   ```sh
   sudo apt install npm
   ```
   This will allow you to install modules and packages to use with Node.js.At this point you have successfully installed Node.js and npm using apt and the default Ubuntu software repositories. 

7. Lets install git using app-get:
   ```sh
   sudo apt-get install git
   ```
   Check that the install was successful by querying git for its version number:
   ```sh
   git --version
   ``` 

8. Clone our node app repo
   ```sh
   git clone https://github.com/<username>/node-app.git

   ```
   Navigate into the project folder and install app dependencies:
   ```sh
   cd node-app
   npm install --production
   ```
   Before we can access our app on the browser we need to complete an additional step. As you recall we are running our app on port 3000 by default. DigitalOcean firewall blocks clients accessing any port but 80. Thankfully Ubuntu includes UFW firewall configuration tool that adds firewall rule to unblock specific ports.

9. Let’s unblock port 7000 and run our app: 
   ```sh
   sudo ufw allow 7000 # this will open port 7000 to access via web
   sudo npm install pm2 -g
   pm2 start index.js --name node-app
   ```

Now our app is running through PM2 as node-app. just got to **http://SERVER.IP:7000** to check that.

# Running Node App Forever
Starting node app like above is good for development purposes but not in production. In case our node instance crash we need a process that will do the auto restart. We are going to use [PM2](https://pm2.keymetrics.io/docs/usage/quick-start/) module to help us with this task. PM2 is a general purpose process manager and a production runtime for Node.js apps with a built-in Load Balancer. Let’s install PM2 and start our node instance:

   ```sh
   sudo npm install pm2@latest -g
   pm2 start index.js
   ```

   Now our node server is configured and running.