# simple-java-maven-app

This repository is for the
[Build a Java app with Maven](https://jenkins.io/doc/tutorials/build-a-java-app-with-maven/)
tutorial in the [Jenkins User Documentation](https://jenkins.io/doc/).

The repository contains a simple Java application which outputs the string
"Hello world!" and is accompanied by a couple of unit tests to check that the
main application works as expected. The results of these tests are saved to a
JUnit XML report.

The `jenkins` directory contains an example of the `Jenkinsfile` (i.e. Pipeline)
you'll be creating yourself during the tutorial and the `scripts` subdirectory
contains a shell script with commands that are executed when Jenkins processes
the "Deliver" stage of your Pipeline.
# Docker in Docker
stop all docker images running on port 8080 or change the port number

1.Go to your cmd run this command
```

docker run -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock --name jenkins jenkins/jenkins:lts

```
we will get an initial admin password do the jenkins installation steps 

After completion of jenkins installation

2.To enter into your docker image run this command
```
docker exec -it -u root jenkins bash

```

3.install docker in docker
```
apt-get update && \
apt-get -y install apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common && \
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
   $(lsb_release -cs) \
   stable" && \
apt-get update && \
apt-get -y install docker-ce
```


now you will be inside your docker and run '''docker ps''' you will get something like this
```
root@cf1fd5908a1c:/# docker ps
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                               NAMES
cf1fd5908a1c        jenkins/jenkins:lts   "/sbin/tini -- /usr/…"   2 hours ago         Up 12 minutes       0.0.0.0:8080->8080/tcp, 50000/tcp   jenkins
root@cf1fd5908a1c:/#   
```
this means you have succesfully created the image 

Now to give jenkins actions access to all users for that run this command
```
chmod 777 /var/run/docker.sock
```
run your jenkins pipeline
```
pipeline{
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }
  }
  stages {
    stage('Build'){
      steps{
        sh 'mvn -B -DskipTests clean package'
      }
     }
    stage('Test'){
      steps{
        sh 'mvn test'
      }
      
  }
  }
}


```
jenkins console output
```
[INFO] Surefire report directory: /var/jenkins_home/workspace/maven-pipeline/target/surefire-reports

-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running com.mycompany.app.AppTest
Tests run: 2, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.033 sec

Results :

Tests run: 2, Failures: 0, Errors: 0, Skipped: 0

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.143 s
[INFO] Finished at: 2021-02-12T09:28:29Z
[INFO] Final Memory: 9M/171M
[INFO] ------------------------------------------------------------------------
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
$ docker stop --time=1 89e4dc8d03b3087474e8c6e6b0911d5c7d5b18138c32d6b371a4123487c42cec
$ docker rm -f 89e4dc8d03b3087474e8c6e6b0911d5c7d5b18138c32d6b371a4123487c42cec
[Pipeline] // withDockerContainer
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```

