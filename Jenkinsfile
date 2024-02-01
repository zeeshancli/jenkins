pipeline {
    agent {
        label "docker"
    }
    stages {
        
        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t https://hub.docker.com/zeeshan321/dockerjenkins:public .'
                }
            }
        }
        stage('Push image to hub') {
            steps {
                script {
                    sh 'docker login -u zeeshan321 -p Zeeshan@100 https://hub.docker.com/zeeshan321'
                    sh 'docker push https://hub.docker.com/zeeshan321/dockerjenkins:public' // Corrected line
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
