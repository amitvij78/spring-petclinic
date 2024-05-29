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
                sh 'mvn clean compile'
            }
        }
        stage ('maven Test') {
            steps {
                sh 'mvn test -Dmaven.test.skip=true'
            }
        }
   }
}
