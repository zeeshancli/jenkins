pipeline {
    agent {
        label "docker"
    }
    stages {
        
        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t zeeshan321/docker-jenkins:public .'
                }
            }
        }
        stage('Push image to hub') {
            steps {
                script {
                    sh 'docker login -u zeeshan321 -p Zeeshan@100 '
                    sh 'docker push zeeshan321/docker-jenkins:public' // Corrected line
                }
            }
        }
        stage('Deploy on K8s Cluster') {
            steps {
                withKubeConfig(caCertificate: "${KUBE_CERT}", clusterName: 'kubernetes', contextName: 'kubernetes-admin@kubernetes', credentialsId: 'my-kube-config-credentials', namespace: 'default', restrictKubeConfigAccess: false, serverUrl: 'https://jump-host:6443') 
                {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl get all'
                }
            }
        }    
    }
}
