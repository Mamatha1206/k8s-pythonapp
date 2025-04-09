pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('98897e03-c137-4438-8da4-dd1f33b63fba')
          KUBECONFIG = "/var/lib/jenkins/.minikube/profiles/minikube/config"// You must create this in Jenkins
       
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/mamatha/k8s-pythonapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t mamatha0124/flask-app:latest .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push mamatha0124/flask-app:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh '''
                        kubectl apply -f deployment.yaml
                        kubectl apply -f service.yaml
                    '''
                }
            }
        }
    }
}
