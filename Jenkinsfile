pipeline {
    agent any
 
    stages {
        stage('Build') {
            steps {
                script {
                    // Build Docker image
                    dockerImage = docker.build('sample-fastapi')
                }
            }
        }
        stage('Test') {
            steps {
                // Run any tests if applicable
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Push Docker image to Docker registry (if needed)
                    dockerImage.push()
                    
                    // Run Docker container
                    docker.withRegistry('https://dockerhub.com', 'docker-hub-credentials') {
                        dockerImage.run('-d -p 8000:8000')
                    }
                }
            }
        }
    }
}
