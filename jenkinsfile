pipeline {
    agent any

    environment {
        // Make sure the PATH includes directories where npm is installed
        PATH = "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
        DOCKER_IMAGE = 'my-web-app:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                // If you're using the default SCM checkout, you can remove this stage.
                // Otherwise, include your checkout steps here.
                git branch: 'main', url: 'git@github.com:fayaz960/my-web-app.git', credentialsId: 'actual-credential-id'
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

