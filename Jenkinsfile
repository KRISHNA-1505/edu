pipeline {
    agent any

    environment {
        IMAGE_NAME = "krishna728/youtube-login-krish-animation"
        TAG = "v1"
        DOCKER_USERNAME = "krishna728"
        DOCKER_PASSWORD = 'rks1505$$$'  // Not secure - avoid using in production
        KUBECONFIG = "/home/vasanth47/.kube/config"  // Set the correct kubeconfig path
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/KRISHNA-1505/edu.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${TAG}")
                }
            }
        }

        stage('Push to Docker Hub') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
            script {
                sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin"
                docker.image("${IMAGE_NAME}:${TAG}").push()
            }
        }
    }
}


        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes deployment and service using kubectl
                    sh 'kubectl apply -f k8s/deployment.yaml --validate=false'
                    sh 'kubectl apply -f k8s/service.yaml --validate=false'
                }
            }
        }
    }
}
