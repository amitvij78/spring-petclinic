pipeline{
    agent any
    tools {
        jdk 'jdk-17'
        maven 'maven-3.9.6'
    }
    stages{
        stage ('clean Workspace'){
            steps{
                cleanWs()
            }
        }
        stage ('checkout code') {
            steps {
                git branch: 'main', poll: false, url: 'https://github.com/amitvij78/spring-petclinic.git'
            }
        }
        stage ('maven compile') {
            steps {
                bat 'mvn clean compile'
            }
        }
        stage ('maven Test') {
            steps {
                bat 'mvn test -Dmaven.test.skip=true'
            }
        }
        stage('Docker build') {
            steps {
                script {
                    def dockerTag = "${env.BUILD_NUMBER}" // Get the current build number
                    def dockerImage = "av78/petclinic_jenkins_repo"
                    withDockerRegistry(credentialsId: 'jenkins-to-docker') {
                        bat "docker build -t ${dockerImage}:${dockerTag} ."
                        bat "docker tag ${dockerImage}:${dockerTag} ${dockerImage}:latest"
                        bat "docker push ${dockerImage}:${dockerTag}"
                        bat "docker push ${dockerImage}:latest"
                    }
                }
            }
        }
   }
}
