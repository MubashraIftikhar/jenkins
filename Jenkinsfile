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
        stage('Verify Files') {
            steps {
                sh 'ls -la'
            }
        }
    }
}
