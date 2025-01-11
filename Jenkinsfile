pipeline {
    agent any

    environment {
        FRONTEND_DIR = 'Frontend'
        BACKEND_DIR = 'Backend'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository
                checkout scm
            }
        }

        stage('Build Frontend') {
            steps {
                script {
                    // Build the frontend
                    dir(FRONTEND_DIR) {
                        sh 'docker build -t react-frontend .'
                    }
                }
            }
        }

        stage('Build Backend') {
            steps {
                script {
                    // Build the backend
                    dir(BACKEND_DIR) {
                        sh 'docker build -t node-backend .'
                    }
                }
            }
        }

        stage('Deploy Containers') {
            steps {
                script {
                    // Deploy the containers using Docker Compose
                    sh 'docker-compose up -d'
                }
            }
        }

        stage('Post-Deployment') {
            steps {
                // Clean up if needed or do any post-deployment actions
                echo 'Deployment complete!'
            }
        }
    }

    post {
        always {
            // Perform any cleanup tasks after the pipeline runs
            echo 'Pipeline finished.'
        }

        success {
            echo 'Pipeline succeeded.'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}
