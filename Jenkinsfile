pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'zeeshan321/jenkinsdocker'
        DOCKER_REGISTRY_URL = 'https://hub.docker.com/repositories/zeeshan321'
        DOCKER_IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    dockerImage = docker.build("${DOCKER_REGISTRY_URL}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to the registry
                    docker.withRegistry("${DOCKER_REGISTRY_URL}", 'docker-credentials-id') {
                        dockerImage.push("${DOCKER_IMAGE_TAG}")
                    }
                }
            }
        }
    


        stage('Deploy to AKS') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'kube-config-credentials-id', variable: 'KUBECONFIG')]) {
                        sh """
                            kubectl config use-context your-aks-context
                            kubectl apply -f deployment.yaml
                        """
                    }
                }
            }
        }
    }
}
