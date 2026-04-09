pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "yamkumar-v/mid-term-web" // Your Docker Hub username/repo
        REGISTRY_CREDS = "docker-hub-creds"     // The ID you set in Jenkins Credentials
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Dockerize') {
            steps {
                script {
                    // This builds the Docker image using your Dockerfile
                    sh "docker build -t ${DOCKER_IMAGE}:latest ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${REGISTRY_CREDS}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // This applies your Kubernetes deployment file
                sh "kubectl apply -f deployment.yaml"
            }
        }
    }
}
