pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker --version'
                sh 'docker build -t my-image .'
            }
        }
    }
    environment {
        FRONTEND_DIR = 'Frontend'
        BACKEND_DIR = 'Backend'
    }

    stages {

        stage('Build Frontend') {
            steps {
                script {
                    // Build the frontend Docker image
                    dir(FRONTEND_DIR) {
                        sh 'docker build -t react-frontend .'
                    }
                }
            }
        }

        stage('Build Backend') {
            steps {
                script {
                    // Build the backend Docker image
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
                // Post-deployment steps, like cleanup
                echo 'Deployment complete!'
            }
        }
    }

    post {
        always {
            // Cleanup actions after the pipeline runs
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
