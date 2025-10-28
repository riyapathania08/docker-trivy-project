pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                echo "Pulling code from GitHub..."
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                bat "docker build -t trivy-app:latest ."
            }
        }

        stage('Trivy Scan') {
            steps {
                echo "Running Trivy vulnerability scan..."
                bat "trivy image --exit-code 1 --severity HIGH,CRITICAL trivy-app:latest"
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Running Docker Container..."
                bat "docker run -d -p 3000:3000 --name trivy-app trivy-app:latest"
            }
        }
    }

    post {
        always {
            echo "Pipeline completed!"
        }
    }
}
