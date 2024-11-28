pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ymokshadadeshmukh17/example-frontend" // Replace with your Docker Hub username
        DOCKER_TAG = "latest" // You can set this to a specific version if needed
    }

    stages {
        stage('Checkout Code') {
            steps {
              checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mokshadadeshmukh15/cicd-vite.git']])
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image using the Dockerfile
                sh """
                docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                // Push the Docker image to Docker Hub
                withCredentials([string(credentialsId: 'docker-hub-credentials-id', variable: 'DOCKER_HUB_PASSWORD')]) {
                    sh """
                    echo "$DOCKER_HUB_PASSWORD" | docker login -u your-dockerhub-username --password-stdin
                    docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image successfully built and pushed to Docker Hub.'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for more details.'
        }
    }
}
