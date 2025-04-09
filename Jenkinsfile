pipeline {
    agent any

    environment {
        IMAGE_NAME = "mamatha0124/flask-app"
        IMAGE_TAG = "v${BUILD_NUMBER}"
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Mamatha1206/k8s-pythonapp.git'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKER_CREDENTIALS_ID', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl set image deployment/flask-app flask-container=${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }
}
