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
                    withCredentials([usernamePassword(credentialsId: 'docker-credential', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'docker login -u $"DOCKER_USER" -p $"DOCKER_PASSWORD" docker.io'
                        sh 'docker push atuldatagithub/test-jenkins:0.1'
                        }
                    }
                }
            }
        }
    }
