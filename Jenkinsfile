pipeline {
    agent any

    environment {
        IMAGE = "yourdockerhubusername/docker-jenkins-demo"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo/docker-jenkins-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE}:latest ."
            }
        }

        stage('Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push ${IMAGE}:latest
                    """
                }
            }
        }
    }

    post {
        always {
            sh "docker logout || true"
        }
    }
}
