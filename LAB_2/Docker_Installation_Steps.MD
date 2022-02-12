# Installing Docker on Amazon Linux server

### Pre-requisites
1. Amazon Linux EC2 Instance

## Installation Steps

1. Install docker and start docker services
   ```sh 
   yum install docker -y
   docker --version 
   
   # start docker services
   service docker start
   service docker status
   ```
1. Change hostname dockerhost and restart the server. 
   ```sh
   vi /etc/hostname
   init 6
   ```  
1. Create a user called dockeradmin
   ```sh
   useradd dockeradmin
   passwd dockeradmin
   ```
1. add a user to docker group to manage docker 
   ```
   usermod -aG docker dockeradmin
   ```
1. Enable password authentication by configuring the PasswordAuthentication directive in your dockerhost server.
   
   ```sh
   vi /etc/ssh/sshd_config
   #Search for PasswordAuthentication and set the option to yes to enable PasswordAuthentication method.

   PasswordAuthentication yes
   ```
1. Reload or restart SSH server service.

   ```
   service sshd reload
   ```
1. If you want to remove the password verification, you can simply add the “NOPASSWD” option.
   ```sh
   dockeradmin       ALL=(ALL:ALL) NOPASSWD:ALL
   ```
1. switch to the newly added user:
   ```sh
   su dockeradmin
   ```
1. Check if the SSH key for the client machine already exists. This will prevent overwriting the current configuration. You can use the below command to find out
   ```sh
   ls -al ~/.ssh/id_*.pub
   ```
1. Next, we can proceed to generate the SSH key
   ```sh
    ssh-keygen 
   ```
- Print the SSH key you just created:

```sh
cat ~/.ssh/id_rsa.pub
```
- This would display an output containing the key starting with ssh-rsa. Copy it! 

- You’ll need to manually add the contents of the id_rsa.pub file to the jenkins server’s root ~/.ssh/authorized_keys file. On the source method you can display the contents of the id_rsa.pub file by using the vi editor or cat command:  login and create the .ssh file if it does not exist.
```sh
mkdir -p ~/.ssh
```
***SSH_public_key*** would be the public key that you copied from the source machine. It will start with ssh-rsa.

- Open the the file where authorized keys are stored:

```sh
vim ~/.ssh/authorized_keys
```

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

### Validation test
1. Create a tomcat docker container by pulling a docker image from the public docker registry
   ```sh
   docker run -d --name test-tomcat-server -p 8090:8080 tomcat:latest
   ```

So far we have completed the installation of docker software to support docker plugin on the jenkins console. Now jump onto your Jenkins to complete the remaining steps. 



