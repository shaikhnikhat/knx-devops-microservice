pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('446914bc-ec4f-4703-8cb3-9f7f4439f8d3')
        IMAGE_NAME = 'niksk/housing-price-predictor'
    }

    stages {
        stage('Git Clone') {
            steps {
                script {
                    git branch: 'main', credentialsId: 'c6c839e0-e759-44a3-ae2b-51f27432f644', url: 'https://github.com/shaikhnikhat/knx-devops-microservice.git'
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                    dockerImage = docker.build("${env.IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Tag Image') {
            steps {
                script {
                    dockerImage.tag('latest')
                }
            }
        }

        stage('Docker Hub Login') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'DOCKERHUB_CREDENTIALS') {
                        sh 'echo "Logged in to Docker Hub"'
                    }
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'DOCKERHUB_CREDENTIALS') {
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh '''
                    docker stop housing-price-predictor || true
                    docker rm housing-price-predictor || true
                    docker run -d --name housing-price-predictor -p 4000:80 ${env.IMAGE_NAME}:latest
                    '''
                }
            }
        }
    }
}

