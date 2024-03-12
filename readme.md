# Containerizing Spring PetClinic Sample Application with Jenkins 

## Objective
Using Open source Spring PetClinic, build a Jenkins Pipeline with STAGES to Compile, Test and build a Runnable Docker Image. 

## Deliverables
1. **Jenkinsfile**: A script defining the pipeline stages
2. **Dockerfile**: Instructions for building a Docker image
3. **Docker-compose.yml**: Configuration for running the application
4. **README.md**: Documentation explaining the setup and usage

## Implementation
Here are the high-level steps:

&ensp;&ensp;***Step 1*** --- Create an Ubuntu EC2 Instance with Jenkins installed  
&ensp;&ensp;***Step 2*** --- Setup Jenkins for PetClinic App  
&ensp;&ensp;***Step 3*** --- Install Docker within EC2 instance  
&ensp;&ensp;***Step 4*** --- Install Plugins - JDK, Maven, Docker on Jenkins  
&ensp;&ensp;***Step 5*** --- Fork PetClinic Repo & Local Cloning  
&ensp;&ensp;***Step 6*** --- Create a Jenkinsfile with pipeline stages  
&ensp;&ensp;***Step 7*** --- Setup a Runnable Docker Container Image for PetClinic Application
&ensp;&ensp;***Step 8*** --- Run the PetClinic Application  


### Step 1 - Create an Ubuntu EC2 Instance with Jenkins Installed
- Launch an EC2 instance with an Ubuntu image
- Configure a Security Group that allows inbound and outbound HTTP and HTTPS traffic
- Optionally restrict access to the IP address from which you'll access Jenkins
- In the 'UserData' section, copy and paste the contents of this file: https://github.com/karunsuresh123/spring-petclinic-jenkins/blob/main/userdata.txt
- This will install Jenkins and bring up the Jenkins Controller, accessible at `http://\<EC2 Public IP Address\>:8080`

### Step 2 - Setup Jenkins for PetClinic App
- Since the PetClinic application runs on port 8080, you must update Jenkins to use port 8090 to avoid conflicts
- Update the configuration files `/etc/default` and `/lib/systemd/system` to reference port 8090 instead of 8080 for Jenkins

This adjustment ensures that both Jenkins and the PetClinic application can coexist without port clashes

### Step 3 - Install Docker within EC2 Instance
- Install Docker using the following command:  
 `sudo apt-get install docker.io -y`
- Add the current user (ubuntu) to the "docker" group, allowing the user to run Docker commands without needing to use `sudo` for every Docker-related operation:  
 `sudo usermod -aG docker ubuntu`
- Additionally, set read-write-execute (RWX) permissions on the 'docker.sock' file to enable non-root user access


### Step 4 - Install Plugins - JDK, Maven, and Docker on Jenkins
1. Go to **Manage Jenkins** ➡️ **Plugins** ➡️ **Available Plugins**
2. Install the following plugins:
    - **JDK**
    - **Maven**
    - **Docker**
3. Configure Java, Maven, and Docker in **Tool Configurations** by setting the required versions


### Step 5 - Fork PetClinic Repo & Local Cloning
1. **Fork the Spring PetClinic Repository**:
    - Visit https://github.com/spring-projects/spring-petclinic and fork the repo
    - Provide a name for your new forked repository (e.g., 'spring-petclinic-jenkins')

2. **Clone the Forked Repository Locally**:
    - Open your terminal or command prompt
    - Run the following command to clone your forked repository to your local machine:
      ```
      git clone https://github.com/your-username/spring-petclinic-jenkins.git
      ```
    - Navigate to the cloned directory
      ```
      cd spring-petclinic-jenkins
      ```
    - Open the project in Visual Studio Code(VSC) or your preferred IDE


### Step 6 - Create a Jenkinsfile with Pipeline stages

In this step, we will create a Jenkinsfile that defines the pipeline stages for building and deploying the Spring PetClinic application.

**<u>Pipeline Stages</u>**

1. Checkout forked 'spring-petclinic-jenkins' from your personal GitHub Repo: `https://github.com/karunsuresh123/spring-petclinic-jenkins.git`

2. Compile the Code

3. Run the Tests

4. Package the Project as a Runnable Docker Image

5. Push the Docker Image to Dockerhub

**<u>Create a New Pipeline within Jenkins</u>**

1. Open Jenkins
2. Create a new pipeline job
3. Select 'Pipeline script from SCM' as the pipeline Definition
4. Set SCM to 'Git'
5. Provide the Git Repository URL: `https://github.com/karunsuresh123/spring-petclinic-jenkins.git`
6. Provide your GitHub credentials



### Step 7 - Setup a Runnable Docker Container Image for PetClinic Application

In this step, we will create a Docker container image for the Spring PetClinic Application. Follow the instructions below:

1. Open the Terminal and Navigate to the Parent Directory of Forked Repo (spring-petclinic-jenkins):
2. Run the Command 'docker init':
   - This will create the necessary files for setting up the Docker container image:
     - `.dockerignore`
     - `Dockerfile`
     - `docker-compose.yml`
     - `README.Docker.md`
3. Update Dockerfile for Multi-Stage Build:
   - Edit the `Dockerfile` to set up a multi-stage build that runs the Spring PetClinic app on port 8080
4. Update docker-compose.yml:
   - Edit the `docker-compose.yml` file to run the PetClinic service connected to a MySQL database service


### Step 8 - Run the PetClinic Application

In this step, we will run the Spring PetClinic application using Docker. Follow the instructions below:

1. *Pre-requisite: You Must Have Docker Host Installed in Your System*

2. Create docker-compose.yml file in your local machine and copy the contents from https://github.com/karunsuresh123/spring-petclinic-jenkins/blob/main/docker-compose.yml to docker-compose.yml

3. Run 'docker compose up' from the location where you have `docker-compose.yml` file:
     ```
     docker compose up --build -d
     ```
   - This will start the PetClinic application and its associated services in Docker containers.

4. View the PetClinic Application in Your Browser:
   - Open your web browser
   - Visit the PetClinic application at http://localhost:8080



