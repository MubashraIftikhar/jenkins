pipeline {
    agent any

    environment {
        IMAGE_NAME = "nginx-demo"
        DOCKERHUB_USERNAME = "mubashraiftikhar10"
    }

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
                sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker tag ${IMAGE_NAME}:${BUILD_NUMBER} $DOCKER_USER/${IMAGE_NAME}:${BUILD_NUMBER}
                        docker push $DOCKER_USER/${IMAGE_NAME}:${BUILD_NUMBER}
                    '''
                }
            }
        }
    }
}

