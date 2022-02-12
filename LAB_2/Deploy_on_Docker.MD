# Deploy on Docker host server using Jenkins 
# *Jenkins Job name:* `Deploy_on_Docker_Host`

### Pre-requisites

1. Jenkins server 
1. Docker-host Server 

### Integration between Docker-host and Jenkins

Install "SSH plugin"
 - `Manage Jenkins` > `Manage Plugins` > `Available` > `SSH`

Jenkins should need access to the tomcat server to deploy build artifacts. setup credentials to enable this process. use credentials option on Jenkins home page.

- setup credentials
  - `credentials` > `jenkins` > `Manage credentials` > `add credentials`
    - Username	: `dockeradmin`
    - Password : `*******`
    - id      :  `dockeradmin`
    - Description: `dockeradmin user`

Enable connection between Docker-host and Jenkins

- `Manage Jenkins` > `Configure System` > `SSH remote hots` > `SSH sites` 
    
		- Hostname: `<ServerIP>`

		- port: `22`
     
    - Credentials: `dockeradmin(dockeradmin user)`
               
- Check connection apply ad save 
 
### Steps to create "Deploy_on_Docker_Host" Jenkin job
 #### From Jenkins home page select "New Item"
   - Enter an item name: `Deploy_on_Docker_Host`
     - Copy from: `Deploy_on_Tomcat_Server`
     
   - *Source Code Management:*
      - Repository: `https://github.com/ValaxyTechDevops/hello-world.git`
      - Branches to build : `*/master`  
   - *Poll SCM* :      - `* * * * *`

   - *Build:*
     - Root POM:`pom.xml`
     - Goals and options: `clean install package`

   - *Post Steps:*
     - *Execute shell scripts on remote host using ssh*
      - SSH site :`dockeradmin@<ipadress>`
      - Command: `scp root@<ipadress>:$WORKSPACE/webapp/target/webapp.war /home/dockeradmin`

Save and run the job now.

log in to docker host cmd, build the docker image with the war file and run the container as follows 

```sh
cd /home/dockeradmin
docker build -t tomcat:v1
docker images 
docker run -d --name tomcatv1 -p 8086:8080 tomcat:v1
```

When complete, you should see your changes on *http://NODE.SERVER.IP:8086/webapp*


