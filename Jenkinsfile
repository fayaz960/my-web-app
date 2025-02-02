pipeline {
    agent any
    
    // Disable the default checkout that Jenkins performs
    options {
        skipDefaultCheckout(true)
    }
    
    environment {
        DOCKER_IMAGE = 'my-web-app:latest'
    }
    
    stages {
        // Explicit checkout stage that uses the correct branch
        stage('Checkout') {
            steps {
                // Use your SSH URL (if that’s how you access your repo) and specify branch 'main'
                // Replace 'your-credential-id' with the actual ID of your SSH credential stored in Jenkins.
                git branch: 'main', 
                    url: 'git@github.com:fayaz960/my-web-app.git', 
                    credentialsId: 'your-credential-id'
            }
        }
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

