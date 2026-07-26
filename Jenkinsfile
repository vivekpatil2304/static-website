pipeline {
    agent any

    environment {
        IMAGE_NAME = "mywebsite"
        CONTAINER_NAME = "mywebsite-container"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME} .
                '''
            }
        }

        stage('Stop and Remove Existing Container') {
            steps {
                sh '''
                if [ "$(docker ps -aq -f name=${CONTAINER_NAME})" ]; then
                    echo "Container ${CONTAINER_NAME} exists."

                    if [ "$(docker ps -q -f name=${CONTAINER_NAME})" ]; then
                        echo "Stopping running container..."
                        docker stop ${CONTAINER_NAME}
                    fi

                    echo "Removing container..."
                    docker rm ${CONTAINER_NAME}
                else
                    echo "No existing container found."
                fi
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                docker run -d \
                    --name ${CONTAINER_NAME} \
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
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully."
        }

        failure {
            echo "Deployment failed."
        }
    }
}
