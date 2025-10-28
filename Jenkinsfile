pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                echo "Pulling code from GitHub..."
                checkout scm
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    bat "echo %PASSWORD% | docker login -u %USERNAME% --password-stdin"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                bat "docker build -t riyapathania08/trivy-app:latest ."
            }
        }

        stage('Trivy Scan') {
            steps {
                echo "Running Trivy vulnerability scan..."
                bat "trivy image --exit-code 0 --severity HIGH,CRITICAL riyapathania08/trivy-app:latest"
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Running Docker Container..."
                bat "docker stop trivy-app || echo 'No container running'"
                bat "docker rm trivy-app || echo 'No container found'"
                bat "docker run -d -p 3000:3000 --name trivy-app riyapathania08/trivy-app:latest"
            }
        }
    }

    post {
        always {
            echo "Pipeline completed!"
        }
    }
}
