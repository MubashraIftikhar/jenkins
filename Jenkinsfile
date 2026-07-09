pipeline {
    agent any
    environment {
        IMAGE_NAME = "nginx-demo"
        DOCKERHUB_USERNAME = "mubashraiftikhar10"
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    try {
                        git branch: 'main',
                            url: 'https://github.com/MubashraIftikhar/jenkins.git',
                            credentialsId: 'github-creds'
                        env.STAGE_CHECKOUT = "PASSED"
                    } catch (e) {
                        env.STAGE_CHECKOUT = "FAILED"
                        throw e
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    try {
                        sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .'
                        env.STAGE_BUILD = "PASSED"
                    } catch (e) {
                        env.STAGE_BUILD = "FAILED"
                        throw e
                    }
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    try {
                        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                            sh '''
                                echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                                docker tag ${IMAGE_NAME}:${BUILD_NUMBER} $DOCKER_USER/${IMAGE_NAME}:${BUILD_NUMBER}
                                docker push $DOCKER_USER/${IMAGE_NAME}:${BUILD_NUMBER}
                            '''
                        }
                        env.STAGE_PUSH = "PASSED"
                    } catch (e) {
                        env.STAGE_PUSH = "FAILED"
                        throw e
                    }
                }
            }
        }
        stage('Remove Local Image') {
            steps {
                script {
                    try {
                        sh '''
                            docker rmi ${IMAGE_NAME}:${BUILD_NUMBER} || true
                            docker rmi ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER} || true
                        '''
                        env.STAGE_REMOVE = "PASSED"
                    } catch (e) {
                        env.STAGE_REMOVE = "FAILED"
                        throw e
                    }
                }
            }
        }
        stage('Pull from Docker Hub') {
            steps {
                script {
                    try {
                        sh 'docker pull ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER}'
                        env.STAGE_PULL = "PASSED"
                    } catch (e) {
                        env.STAGE_PULL = "FAILED"
                        throw e
                    }
                }
            }
        }
        stage('Run Container') {
            steps {
                script {
                    try {
                        sh '''
                            docker rm -f nginx-demo-run || true
                            docker run -d -p 8082:80 --name nginx-demo-run ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${BUILD_NUMBER}
                        '''
                        env.STAGE_RUN = "PASSED"
                    } catch (e) {
                        env.STAGE_RUN = "FAILED"
                        throw e
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                echo """
========================================
        PIPELINE STAGE SUMMARY
========================================
| Stage                  | Status     |
|-------------------------|-----------|
| Checkout                | ${env.STAGE_CHECKOUT ?: 'SKIPPED'} |
| Build Docker Image      | ${env.STAGE_BUILD ?: 'SKIPPED'} |
| Push to Docker Hub      | ${env.STAGE_PUSH ?: 'SKIPPED'} |
| Remove Local Image      | ${env.STAGE_REMOVE ?: 'SKIPPED'} |
| Pull from Docker Hub    | ${env.STAGE_PULL ?: 'SKIPPED'} |
| Run Container           | ${env.STAGE_RUN ?: 'SKIPPED'} |
========================================
Overall Result: ${currentBuild.result ?: 'SUCCESS'}
========================================
"""
            }
        }
    }
}
