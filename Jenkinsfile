pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub' 
        DOCKERHUB_REPO = 'shaimaa9/docker-integration'  
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    def imageTag = "${env.JOB_NAME}:${env.BUILD_NUMBER}"
                    sh """
                        docker build -t ${DOCKERHUB_REPO}:${imageTag} .
                    """
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", 
                                                     usernameVariable: 'DOCKER_USER', 
                                                     passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    def imageTag = "${env.JOB_NAME}:${env.BUILD_NUMBER}"
                    sh "docker push ${DOCKERHUB_REPO}:${imageTag}"
                }
            }
        }
    }
}
