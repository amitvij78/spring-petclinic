pipeline{
    agent any
    tools {
        jdk 'jdk18'
        maven 'maven3'
    }
    stages{
        stage ('clean Workspace'){
            steps{
                cleanWs()
            }
        }
        stage ('checkout scm') {
            steps {
                git branch: 'main', credentialsId: 'github_token', poll: false, url: 'https://github.com/karunsuresh123/spring-petclinic-jenkins.git'
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
        stage ('Build war file'){
            steps{
                sh 'mvn clean install -DskipTests=true'
            }
        }
        stage('Docker build') {
            steps {
                script {
                    def dockerTag = "${env.BUILD_NUMBER}" // Get the current build number
                    withDockerRegistry(credentialsId: 'docker') {
                        sh "docker build -t karunsureshraj/petclinic_jenkins_repo:${dockerTag} ."
                        sh "docker push karunsureshraj/petclinic_jenkins_repo:${dockerTag}"
                    }
                }
            }
        }
   }
}
