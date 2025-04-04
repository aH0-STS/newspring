pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'rohith1305/java-application:latest'
        DOCKER_CREDENTIALS = '7c910bc4-e2e4-48a4-857c-51be93277e96'  // Jenkins Credentials ID for Docker login
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Checkout the code from GitHub repository
                    git branch: 'main', url: 'https://github.com/KyathamRohith/springboot.git'
                }
            }
        }

        stage('Build with Maven') {
            steps {
                script {
                    // Build the application with Maven (skip tests)
                    sh 'mvn clean package -DskipTests=true'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub using Jenkins credentials
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        sh "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Ensure kubectl is configured correctly on the Jenkins agent
                    sh "kubectl apply -f deployment.yaml"
                    sh "kubectl apply -f service.yaml"
                }
            }
        }
    }
}
