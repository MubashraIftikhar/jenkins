pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/MubashraIftikhar/jenkins.git',
                    credentialsId: 'github-creds'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t nginx-demo:${BUILD_NUMBER} .'
            }
        }
    }
}
