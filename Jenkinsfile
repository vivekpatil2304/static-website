pipeline {
    agent any

    environment {
        IMAGE_NAME = "mywebsite"
        CONTAINER_NAME = "mywebsite-container"
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                echo "Checking out source code from GitHub..."
                checkout scm
            }
        }

        stage('Verify Workspace') {
            steps {
                sh '''
                    echo "Current Directory:"
                    pwd

                    echo "Workspace Files:"
                    ls -la

                    echo "Docker Version:"
                    docker version
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    export DOCKER_BUILDKIT=0

                    echo "Building Docker Image..."

                    docker build -t ${IMAGE_NAME} .
                '''
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh '''
                    echo "Stopping old container if it exists..."

                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                    echo "Starting new container..."

                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        --restart unless-stopped \
                        -p 80:80 \
                        ${IMAGE_NAME}
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    echo "Running Containers:"
                    docker ps

                    echo "Docker Images:"
                    docker images
                '''
            }
        }
    }

    post {
        success {
            echo "===================================="
            echo "Deployment Successful!"
            echo "Website is running on Port 80"
            echo "===================================="
        }

        failure {
            echo "===================================="
            echo "Deployment Failed!"
            echo "Check Console Output for details."
            echo "===================================="
        }

        always {
            cleanWs()
        }
    }
}
