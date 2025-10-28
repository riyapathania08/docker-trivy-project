pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                echo "Pulling code from GitHub..."
                git 'https://github.com/riyapathania08/docker-trivy-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                bat 'docker build -t trivy-app:latest .'
            }
        }

        stage('Run Docker Scan with Trivy') {
            steps {
                echo "Scanning Docker image with Trivy..."
                bat '''
                trivy image --exit-code 1 --severity HIGH,CRITICAL trivy-app:latest
                '''
            }
        }

        stage('Run Docker Container') {
            when {
                expression { currentBuild.result == null } // Only run if previous steps succeed
            }
            steps {
                echo "Running Docker Container..."
                bat 'docker run -d -p 3000:3000 --name trivy-app trivy-app:latest'
            }
        }
    }

    post {
        always {
            echo "Pipeline completed!"
        }
    }
}
