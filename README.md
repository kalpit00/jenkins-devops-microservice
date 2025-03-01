# Jenkins CI/CD Pipeline with Docker & Java Maven  

This project demonstrates setting up a **Jenkins CI/CD pipeline** to automate a **Java Spring Boot** application using **Docker** and **Maven**.  

## Prerequisites  

To run Jenkins locally, you need:  
- **Docker** installed  
- **Jenkins** running inside a Docker container  
- A **DockerHub account** (for pushing images)  

---  

## Step 1: Install and Run Jenkins in Docker  

Jenkins is installed using Docker to run locally. Follow the official installation guide:  
🔗 [Jenkins Docker Installation](https://www.jenkins.io/doc/book/installing/docker/#on-macos-and-linux)  

### Steps to Run Jenkins  
1. Create a `Dockerfile` in the **root directory** of the Spring Boot application.  
2. Use a `Docker:dind` image to build and run Jenkins.  
3. Start Jenkins inside a Docker container (configured on port **8081**).  

Once running, open **`http://localhost:8081`** in a browser.  

### Retrieve Jenkins Initial Admin Password  
1. Run:  
   ```sh
   docker container ls
   ```  
   Find the **Container ID** of the Jenkins instance.  
2. Retrieve logs:  
   ```sh
   docker logs <containerId>
   ```  
3. Copy the `initialAdminPassword` from the logs.  
4. Use this password for the first-time login and set up a username-password for future access.  

---  

## Step 2: Configuring Jenkins Pipeline  

Jenkins pipelines are defined in a `Jenkinsfile`. This script automates the build process.  

### Declarative vs. Scripted Pipelines  
- **Declarative Pipeline** (Recommended) → **Auto-pulls** commits from SCM (e.g., GitHub).  
- **Scripted Pipeline** → More flexible but requires manual control over build steps.  

### GitHub Integration  
- Any **committed or pushed changes** trigger a new Jenkins build.  
- A **Scheduler** is set for **1-minute intervals**, ensuring a new pipeline run after every commit.  

---  

## Step 3: Setting Up Maven & Docker in Jenkins  

To run the Java Spring Boot application within Jenkins, enable:  
1. **Apache Maven** → Add via **Jenkins Global Tool Configuration**.  
2. **Docker** → Enable Docker commands in Jenkins.  

Jenkins now supports `mvn` and `docker` commands inside the pipeline script.  

### **Important:** Add DockerHub Credentials  
To enable `docker build` and `docker push` in Jenkins:  
- Go to **Jenkins Global Credentials**.  
- Add your **DockerHub username and password**.  
- Use `docker.withRegistry()` to authenticate.  

---  

## Step 4: Jenkins Pipeline Stages  

The **Jenkins pipeline** consists of the following stages:  

1️⃣ **Compile Maven Project**  
   ```sh
   sh "mvn clean compile"
   ```  

2️⃣ **Run Tests & Integration Tests**  
   ```sh
   sh "mvn test"
   sh "mvn failsafe:integration-test failsafe:verify"
   ```  

3️⃣ **Package JAR File**  
   ```sh
   sh "mvn package -DskipTests"
   ```  

4️⃣ **Build & Push Docker Image**  
   ```sh
   docker build -t <docker_image_path>:$env.BUILD_TAG .
   ```  
   Push to **DockerHub**:  
   ```sh
   docker.withRegistry('', 'dockerhub') {
     dockerImage.push();
     dockerImage.push('latest');
   }
   ```  

---  

## Summary  

This Jenkins pipeline automates:  
✅ **Building a Java Spring Boot application** using Maven  
✅ **Running unit & integration tests**  
✅ **Packaging the application** into a JAR file  
✅ **Creating a Docker image** and pushing it to DockerHub  

Explore different Jenkins pipeline configurations to enhance CI/CD automation! 🚀  
