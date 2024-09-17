pipeline {
    agent any

    environment {
        registry = "aryamannkhare8/dso-lab-6"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }

    stages {
        stage('Cloning Git') {
            steps {
                git url: 'https://github.com/aryamannkhare8/dso-lab-6.git', branch: 'main'
            }
        }
        stage('Building Docker Image') {
            steps {
                script {
                    dockerImage = docker.build registry
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                
                    // Example using Trivy for security scan
                    sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image ${registry}'
                }
            }
        }
        stage('Deploying Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Clean up') {
            steps {
                sh "docker rmi ${registry}"
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

