pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS = credentials('321a47fe-725d-4a11-9c08-44fc1a764a88') // Assuming you've set up the credentials in Jenkins
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
                    withCredentials([usernamePassword(credentialsId: '321a47fe-725d-4a11-9c08-44fc1a764a88', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
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
