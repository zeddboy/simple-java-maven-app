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
#Docker in Docker
stop all docker image running on port 8080 or chanege the port number
1.Go to your comd run this command
'''
docker run -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock --name jenkins jenkins/jenkins:lts
'''
2.To enter into your docker image run this command
'''
docker exec -it -u root jenkins bash
'''
now you will be inside your docker and run '''docker ps''' you will see something like this
'''
root@cf1fd5908a1c:/# docker ps
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                               NAMES
cf1fd5908a1c        jenkins/jenkins:lts   "/sbin/tini -- /usr/…"   2 hours ago         Up 12 minutes       0.0.0.0:8080->8080/tcp, 50000/tcp   jenkins
root@cf1fd5908a1c:/#   
'''
this means you have succesfully created the image 
Now i will give actions access to all users 
'''
chmod 777 /var/run/docker.sock
'''
after this your pipeline in jekinsfile
