pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub'
        IMAGE_NAME = "riyapathania08/trivy-app"
    }

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
                echo "Scanning Docker image for vulnerabilities..."
                bat "trivy image --exit-code 0 --severity HIGH,CRITICAL %IMAGE_NAME%:latest"
            }
        }

        stage('Docker Login') {
            steps {
                      withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            bat "echo %PASSWORD% | docker login -u %USERNAME% --password-stdin"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo "Pushing image to Docker Hub..."
                bat "docker push %IMAGE_NAME%:latest"
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Running Docker Container..."
                bat "docker run -d -p 3000:3000 --name trivy-app %IMAGE_NAME%:latest"
            }
        }
    }

    post {
        always {
            echo "Pipeline completed!"
        }
    }
}
