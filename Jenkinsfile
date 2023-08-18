pipeline {
    agent { label 'dev-agent' }
    
    stages{
        stage('Code'){
            steps {
                git url: 'https://github.com/harshmandhu/DevOps-Project-node-todo-cicd.git', branch: 'master'
            }
        }
        stage('Build and Test'){
            steps {
                sh 'docker build . -t harshmandhu/node-todo-app-cicd:latest' 
            }
        }
        stage('Push Image'){
            steps {
                echo 'logging in to docker hub and pushing image..'
                withCredentials([ credentialsId: "docker-cred", url: ""]) {
                     dockerImage.push()
                }
            }
        }
    }
}
