pipeline {
    agent any

    environment {
        // Define the Docker image name
        DOCKER_IMAGE = 'my-web-app:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull code from Git
                git 'https://github.com/fayaz960/my-web-app.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install Node.js dependencies
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                // Run tests using npm script (assumes tests are defined)
                sh 'npm test'
            }
        }
        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Deploy') {
            steps {
                // Stop any existing container (ignore errors if it doesn't exist)
                sh 'docker stop my-web-app-container || true'
                sh 'docker rm my-web-app-container || true'
                // Run the new container in detached mode
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

