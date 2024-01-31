pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials-id') // Use the ID of Jenkins credentials storing Docker Hub username and password
        DOCKER_IMAGE_NAME = "zeeshan321/jenkins-docker"
        KUBE_CONFIG = credentials('kube-config-credentials-id') // Use the ID of Jenkins credentials storing your AKS kubeconfig
        KUBE_NAMESPACE = "default"
        KUBE_DEPLOYMENT_NAME = "kubectl"
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
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_HUB_CREDENTIALS) {
                        def customImage = docker.build(DOCKER_IMAGE_NAME)
                        customImage.push()
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
