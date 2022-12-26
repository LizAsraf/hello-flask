# Jenkins Master and Slave configuration

## 🧰 Prerequisites
Two aws EC2 instances as cloud infrastructure service.
each Operating system deployed as a Ubuntu system. 
One instance Named ‘master’ and the second ‘slave’

## Procedure: 
1. On ‘master’ - installed containerized Jenkins server
	you will be able to enter to the system as a builder user with username ‘builder’ and password ‘Otom8oEx’

2. On ‘slave’- there is a user inside the system called builder that the master can SSH connect to with no password requirements.
	The jenkins slave also containerized
   
3. There is a job called hello-flask on the Jenkins UI that can be run manually through the UI
- This job running only with the builder agent (slave OS)
- After the job completed successfully the finished artifact called hello-${BUILD_NUMBER}.tar.gz stored in the master container

file path : 
**$JENKINS_HOME/jobs/$your_job_name/branches/$branch_name/builds/$build_number/archive/$file_name.tar.gz**

also you will be able to find the fingerprint and the last successful build artifact on the 
**UI under the branch master above the jobs**
  
## Contributing
To run this server required to run this code with master-ip argument and slave-ip argument
**note:** for this app the slave container supposed to be with python, the referenced container is without python, 
it is required to create a new docker file with this base file or to manually install python on it.

```
#!/bin/sh
master=$1
slave=$2
path_to_key=$3
workDir="/home/jenkins/agent"
ssh -i "${path_to_key}" ec2-user@${master} \
"docker run -d -v jenkins_home:/var/jenkins_home -p 8080:8080 -p 50000:50000 --restart=on-failure jenkins/jenkins:lts-jdk11"
echo "jenkins server is up and running"
ssh -i "${path_to_key}" ec2-user@${slave} \
"docker run -i --name agent --init jenkins/agent java -jar /usr/share/jenkins/agent.jar;\
echo curl -sO http://${master}:8080/jnlpJars/agent.jar > slave.sh ;\
echo java -jar agent.jar -jnlpUrl http://${master}:8080/manage/computer/builder/jenkins-agent.jnlp -secret e1b175440b3c0148cac24af2bb0c617b2c23f68fc4ceb9310d79384b09b4ce8d -workDir ${workDir} >> slave.sh;\
chmod +x slave.sh;\
docker cp slave.sh builder:/home/jenkins/slave.sh;\
docker exec -id builder bash slave.sh;"
echo "jenkins slave is up and running"
```

required to make it executable

```
chmod +x run jenkins.sh
```

```
./jenkins.sh xx.xx.xx.xx yy.yy.yy.yy
```
  
## 🔗 Infra-As-Code for a easier deployment
[![portfolio](https://img.shields.io/badge/infrastructure-000?style=for-the-badge&logo=ko-fi&logoColor=white)](https://github.com/LizAsraf/Infra-As-Code-jenkins-master-slave.git) 