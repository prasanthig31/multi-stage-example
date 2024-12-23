pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'prasanthig/multistage' // Replace with your Docker Hub repo
        DOCKER_CREDENTIALS = 'dockerhub' // Replace with your Jenkins credential ID
        IMAGE_TAG = 'latest' // You can modify this as needed (e.g., ${env.BUILD_NUMBER})
    }
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                    echo "Building Docker Image"
                    docker build -t ${DOCKER_IMAGE}:${IMAGE_TAG} .
                    '''
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS) {
                        echo "Logged in to Docker Hub"
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    sh '''
                    echo "Pushing Docker Image to Docker Hub"
                    docker push ${DOCKER_IMAGE}:${IMAGE_TAG}
                    '''
                }
            }
        }
    }
    post {
        success {
            echo 'Docker image built and pushed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
