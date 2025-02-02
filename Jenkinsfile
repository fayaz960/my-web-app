pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my-web-app:latest'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker stop my-web-app-container || true'
                sh 'docker rm my-web-app-container || true'
                sh 'docker run -d --name my-web-app-container -p 3000:3000 $DOCKER_IMAGE'
            }
        }
    }
    post {
        failure {
            echo 'The pipeline has failed!'
        }
    }
}

