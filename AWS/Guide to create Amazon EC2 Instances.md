## The Beginner’s Guide to create Amazon EC2 Instances

This tutorial will show you how to create EC2 instances from scratch.

### Requirements:

1 x AWS account (if it’s an IAM user verify that you have permission to launch EC2 instances)

### Instructions:

### *Step 1*

First of all, we need to login into the AWS Console, access using this URL: *https://aws.amazon.com/console* and you are going to see this page, click the button “Sign In to the Console.”

<img src="/images/aws_sign_in1.png">

### *Step 2*.

It’s going to open a page requesting your username or email. Type your password.

<img src="/images/aws_sign_in2.png">

### *Step 3*

This is what the AWS console looks like:

<img src="/images/aws_console_look.png">

### *Step 4*

Click on EC2.

<img src="/images/aws_select_ec2.png">

### *Step 5*

This is how the EC2 Dashboard looks.

<img src="/images/aws_ec2_dashboard.png">

### *Step 6*

Click the big blue button “Launch Instance.”

<img src="/images/aws_launch_instance.png">

### *Step 7*

Immediately after you click the button a list of AMIs is going to appear. There you can select the one that you prefer, in this case, I’m going to choose Ubuntu Server 16.04 LTS.

### *Step 8*

After you click the select button of your chosen AMI, you are going to be able to select the EC2 instance type from the list, for this example, I’m going to use t2.micro, select and the next button.

<img src="/images/aws_ec2_intance_type.png">

### *Step 9*

The next page is going to show you some options to set up your instance. For the moment, just be sure that the Auto-assign Public IP it’s set to enable and click next.

<img src="/images/aws_ec2_configure _instance_details.png">

### *Step 10*

Now, on this page, you can choose the amount of GB that your hard drive it’s going to have.

Note: Never leave the default 8gb, if you want to be on the free tier limits you can set a value around 20gb -24gb, because sometimes you leave it as default and your instance is not going to have too many spaces to do many things, and click next.

<img src="/images/aws_ec2_gib_size.png">

### *Step 11*

You should be on the Tags screen.

<img src="/images/aws_ec2_add_tags_1.png">

### *Step 12*

Let’s click on the button “Add Tag” and on the key column type “Name” and on the value type the name that you want to give to your instance and click on next.

<img src="/images/aws_ec2_add_tags_2.png">

### *Step 13*

Setup a Security Group: A Security group it’s like the firewall for your instance. Here you have to open the ports that you are going to use: i.e., if you want to have a web server you need to open the port 80 if you want ssh access you need the port 22, so let’s create a new one.

<img src="/images/aws_ec2_security_group_name.png">

<img src="/images/aws_ec2_key_pair.png">

### *Step 14*

Let’s give a name and description to the new Security Group.

<img src="/images/aws_ec2_security_group_name.png">

### *Step 15*

Ports: The port 22 (ssh) open world (0.0.0.0/0) is added by default; we can leave it like that or choose “my IP” on the list of the source column. As a security measure, AWS only open ssh port to the ones that you trust and don’t leave it open world, add a description to identify and click on “Review and Launch.”

<img src="/images/aws_ec2_configure_security_group_2.png">

### *Step 16*

A Summary of all the setting that you have set it’s going to be shown on screen if everything looks fine, click on Launch, if not you can return to change everything that you need.

<img src="/images/aws_ec2_launch.png">

### *Step 17*

A key? This little thing is what you need to be able to access your brand new instance. Give it a name and click the button “Download Key Pair.” Then, save that file because you are not going to be able to get it in another time. AWS doesn’t store keys, so if you lose it, you are going to lose access to your instance.

### *Step 18*

Click on “Launch Instances.”

<img src="/images/aws_ec2_launch_instance.png">

If everything goes well you are going to see the next screen, click on the id to view it on the dashboard.

<img src="/images/aws-ec2_instance_status.png">


### *Step 19*

Access to the instance

We are going to connect to AWS EC2 Instance using MobaXTerm. (Download and install MobaXTerm from official MobaXTerm site for Windows.)
*https://mobaxterm.mobatek.net/download-home-edition.html*

- Once Installed open the mobaxterm, Go to **Sessions** Menu , click on **New Session** as shown below.

<img src="/images/mobaxterm_launch_new_session.png">

- Input your AWS EC2 Instance IP , UserName in **Remote Host** and **Specify UserName** field.

- Here I am connecting to Ubuntu Instance , If your are connecting AWS EC2 then type user **ec2-user** into the field Specify username

- Click on **Advanced SSH Settings** tab and give the path of your instance .pem file in field **use Private key** as shown below

- Click on **OK** button to connect AWS EC2 instance using mobaxterm

<img src="/images/mobaxtern_connect_ec2_instance.png">

- Once connected , you will see like below AWS Instance SSH terminal

<img src="/images/monaxterm_instance_terminal.png">


## And you are in?


