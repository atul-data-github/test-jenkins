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
                    dockerImage = docker.build('sample-fastapi')
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Push Docker image to Docker registry
                    withCredentials([usernamePassword(credentialsId: 'docker-credential', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
                        docker.withRegistry('https://dockerhub.com', DOCKER_USER, DOCKER_PASSWORD) {
                            dockerImage.push()
                    
                            // Run Docker container
                            dockerImage.run('-d -p 8000:8000')
                        }
                    }
                }
            }
        }
    }
}
