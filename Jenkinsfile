pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = 'Abhi@1234'
        DOCKER_HUB_REPO = 'abhimanyu2808'
    }
    stages {
        stage('Build Docker Images') {
            steps {
                script {
                    docker.build("${DOCKER_HUB_REPO}/frontend:latest", "-f frontend/Dockerfile frontend")
                    docker.build("${DOCKER_HUB_REPO}/backend:latest", "-f backend/Dockerfile backend")
                }
            }
        }
        stage('Push Docker Images') {
            steps {
                script {
                    docker.withRegistry('', DOCKER_CREDENTIALS_ID) {
                        docker.image("${DOCKER_HUB_REPO}/frontend:latest").push()
                        docker.image("${DOCKER_HUB_REPO}/backend:latest").push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    sh 'kubectl apply -f k8s/'
                }
            }
        }
    }
}

