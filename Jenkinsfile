pipeline {
    agent any
    parameters {
        string(name: 'IMAGE_TAG', defaultValue: '', description: 'Docker image tag to pull and run (leave empty to use this build\'s own tag)')
    }
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
                sh 'docker build -t ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER} .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER}
                    '''
                }
            }
        }
        stage('Determine Tag to Pull') {
            steps {
                script {
                    env.TAG_TO_USE = params.IMAGE_TAG?.trim() ? params.IMAGE_TAG.trim() : env.BUILD_NUMBER
                    echo "Will pull and run tag: ${env.TAG_TO_USE}"
                }
            }
        }
        stage('Remove Local Image') {
            steps {
                sh '''
                    docker rmi ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${TAG_TO_USE} || true
                '''
            }
        }
        stage('Pull from Docker Hub') {
            steps {
                sh 'docker pull ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${TAG_TO_USE}'
            }
        }
        stage('Run Container') {
            steps {
                sh '''
                    docker rm -f nginx-demo-run || true
                    docker run -d -p 8082:80 --name nginx-demo-run ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${TAG_TO_USE}
                '''
            }
        }
    }
}
