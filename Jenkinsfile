pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS = credentials('docker-credential')
    }
    
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'docker build -t atuldatagithub/test-jenkins:$(git rev-parse --short HEAD) .'
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR docker.io --password-stdin'
                    sh 'docker push atuldatagithub/test-jenkins:$(git rev-parse --short HEAD)'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig-minikube', variable: 'KUBECONFIG_PATH')]) {
                    script {
                        sh '''
                            export KUBECONFIG=$KUBECONFIG_PATH
                            
                            # Patch deployment with latest image tag
                            IMAGE_TAG=$(git rev-parse --short HEAD)
                            sed -i "s|atuldatagithub/test-jenkins:latest|atuldatagithub/test-jenkins:$IMAGE_TAG|" k8s/deployment.yaml
                            
                            # Apply manifests
                            kubectl apply -f k8s/deployment.yaml
                            kubectl apply -f k8s/service.yaml
                        '''
                    }
                }
            }
        }
    }
}
