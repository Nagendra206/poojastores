pipeline {
    agent {
        label 'docker' // This assumes you have a Jenkins agent with Docker capabilities labeled as 'docker'
    }
    environment {
        // Define environment variables as needed
        DOCKER_IMAGE = "poojastores:latest"
        CONTAINER_NAME = "pooja_stores"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Restore Dependencies') {
            steps {
                // Restore .NET application dependencies (e.g., NuGet packages)
                sh 'dotnet restore'
            }
        }
        
        stage('Build') {
            steps {
                // Build the .NET application
                sh 'dotnet build'
            }
        }
        
        stage('Test') {
            steps {
                // Run tests (optional, adjust as needed)
                sh 'dotnet test'
            }
        }
        
        stage('Publish') {
            steps {
                // Publish the .NET application for deployment
                sh 'dotnet publish -o ./publish'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                script {
                    docker.build("${DOCKER_IMAGE}", "-f Dockerfile ./publish")
                }
            }
        }
        
        stage('Run Docker Container') {
            steps {
                // Stop and remove the existing container if it's running
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"

                // Run the Docker container
                sh "docker run -d -p 8080:80 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}"
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
