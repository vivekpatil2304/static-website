pipeline {

    agent any

    stages {

        stage('Clone') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t mywebsite .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker rm -f mywebsite || true
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker run -d -p 80:80 --name mywebsite mywebsite
                '''
            }
        }

    }
}
