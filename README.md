Jenkins CI/CD pipeline created using Docker + Java Maven

My First Jenkins Pipeline to automate a simple Java Spring Boot application

Jenkins installed using Docker, so running the Jenkins on my local machine. Follow these steps to run Jenkins locally :
https://www.jenkins.io/doc/book/installing/docker/#on-macos-and-linux

Configure a DockerFile in the `root` directory where the SpringBoot application is located, and copy the code for a `Docker:dind` image to build and run the Jenkins instance

Once Jenkins runs inside a Docker container (I configured it on port 8081), open the localhost:8081 in a browser. In order to find the initial password, you can follow
https://www.jenkins.io/doc/book/system-administration/viewing-logs/
run `docker container ls` and grab the container id of the Jenkins image, and run `docker logs <containerId>`

Copy the `initialAdminPassword` as Jenkins will ask this to sign up for the first time. It will ask to setup a Username-Password as well, and this becomes your credentials to login to this image of Jenkins on your machine


Once Jenkins is launched, all the work is done using the `JenkinsFile`

This is the file where we write Jenkins script. There are two main ways Jenkins DSL interprets it, one is `Scripted`, other is `Declarative`

The `Declarative` Syntax `AUTO-PULLS` the commits from the SCM. Therefore, you can work on this JenkinsFile in your IDE and any changes that are pushed/commited to the version control (I used Github), Jenkins will initialize a new "Run" of the pipeline

I configured the `Scheduler` for 1 minute, so Jenkins sends out a new pipeline within a minute after every commit to the Github repo


To the Actual Jenkins CI/CD Pipeline, I played around with the Script (this is quite similar to Azure Devops or other cloud based pipelines and syntax is quite Yaml-like)

To run the actual Java Spring Boot application on Jenkins, we need to enable Maven and Docker on the Jenkins server

1. Add Apahce Maven tool in Jenkins Settings.
2. Add Docker tool in Jenkins Settings. Now we can use `mvm` commands or `docker` commands as part of our Jenkins Script


IMPORTANT STEP : Add your DockerHub Credentials to Jenkins Global Credentials page in order to use `docker build` in the script. We will use `docker.withRegistry` and push the docker image directly to our dockerHub

JENKINS PIPELINE
1. Compile Maven Project Stage
2. Test + Integration Tests Stage
3. Build/Package Jar File Stage
4. Build & Push Docker Image Stage

A quite straightforward Jenkins Pipeline. We will first compile our Java Spring Boot application using `sh "mvn clean compile"`

Then we will run the Integration tests using `sh "mvn test"` and `sh "mvn failsafe:integration-test failsafe:verify"`

Package the Jar file by `sh "mvn package -DskipTests"`


Finally, build the docker image using `"docker build -t <docker_image_path>:$env.BUILD_TAG"`

And push the docker image to dockerhub using :

`docker.withRegistry('', 'dockerhub') {
  dockerImage.push();
  dockerImage.push('latest');
}`
