pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-app'
        DOCKER_HUB_USER = 'your-dockerhub-username' // Optional if pushing
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning source code..."
                // Jenkins does this automatically if connected to Git
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                script {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Run Container (Optional)') {
            steps {
                echo "Running container..."
                script {
                    sh 'docker run -d -p 5000:5000 --name flask-container $IMAGE_NAME'
                }
            }
        }

        // Optional: Push to Docker Hub
        stage('Push to Docker Hub (Optional)') {
            when {
                expression { return false } // Set to true if you want to enable it
            }
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials-id') {
                        sh "docker tag $IMAGE_NAME $DOCKER_HUB_USER/$IMAGE_NAME:latest"
                        sh "docker push $DOCKER_HUB_USER/$IMAGE_NAME:latest"
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning up..."
            sh 'docker rm -f flask-container || true'
        }
    }
}
