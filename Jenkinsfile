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
                    sh 'docker build -t atuldatagithub/test-jenkins:$(git rev-parse --short HEAD) .'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Push Docker image to Docker registry
                        sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR  docker.io --password-stdin'
                        sh 'docker push atuldatagithub/test-jenkins:$(git rev-parse --short HEAD)'
                        sh 'docker stop $(docker ps --quiet --filter "label=atuldatagithub/test-jenkins") && docker run -d --expose 8000 --label atuldatagithub/test-jenkins -p 8000:8000 atuldatagithub/test-jenkins:$(git rev-parse --short HEAD) || docker run -d --expose 8000 --label atuldatagithub/test-jenkins -p 8000:8000 atuldatagithub/test-jenkins:$(git rev-parse --short HEAD)'
                    }
                }
            }
        }
    }
