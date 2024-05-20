pipeline {
    agent any

    stages {
        stage('Build Image') {
            when {
                branch 'main'  // run these steps only on the main branch
            }
            steps {
                script {
                    def app = docker.build("yourdockerhubusername/housing-prediction-app:${env.BUILD_ID}")
                }
            }
        }

        stage('Publish Image') {
            when {
                branch 'main'  // run these steps only on the main branch
            }
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        app.push('latest')
                        app.push("${env.BUILD_ID}")
                    }
                }
            }
        }
    }
}

