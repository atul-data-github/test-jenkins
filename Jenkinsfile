pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS = credentials('docker-credential') // Assuming you've set up the credentials in Jenkins
    }
    
    stages {
        stage('Build') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t atuldatagithub/test-jenkins:0.1 .'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Push Docker image to Docker registry
                        sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR  docker.io'
                        sh 'docker push atuldatagithub/test-jenkins:0.1'
                        
                    }
                }
            }
        }
    }
