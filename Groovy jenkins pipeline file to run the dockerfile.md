pipeline {
    agent any
    
    environment {
        // Replace with your DockerHub or private container registry details
        REGISTRY_USER = 'your-dockerhub-username'
        IMAGE_NAME    = 'spring-aop-app'
        IMAGE_TAG     = "${BUILD_NUMBER}" // Uses Jenkins build number as tag
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Triggers Docker to read the multi-stage Dockerfile
                    sh "docker build -t ${REGISTRY_USER}/${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }
        
        stage('Push to Registry') {
            steps {
                script {
                    // Securely injects Docker login credentials stored in Jenkins
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh "echo ${PASS} | docker login -u ${USER} --password-stdin"
                        sh "docker push ${REGISTRY_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }
}