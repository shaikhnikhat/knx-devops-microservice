pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'git-login', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git-login', url: 'https://github.com/shaikhnikhat/knx-devops-microservice.git']])
                }
            }
        }
        stage('Build/Tag Docker Image') {
            steps {
                script {
                    sh 'docker build -t knx-microservice-app:v1 .'
                    sh 'docker tag knx-microservice-app:v1 niksk/knx-microservice-app:v1'
                }
            }
        }
        stage('Push Image To DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-login', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                        sh 'docker push niksk/knx-microservice-app:v1'
                    }
                }
            }
        }
    }
}

