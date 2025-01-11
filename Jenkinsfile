pipeline {
    agent any

    environment {
        FRONTEND_DIR = 'Frontend'
        BACKEND_DIR = 'Backend'
    }

    stages {
        stage('Build Frontend') {
            steps {
                script {
                    dir(env.FRONTEND_DIR) {
                        bat 'docker build -t react-frontend .'
                    }
                }
            }
        }

        stage('Build Backend') {
            steps {
                script {
                    dir(env.BACKEND_DIR) {
                        bat 'docker build -t node-backend .'
                    }
                }
            }
        }

        stage('Deploy Containers') {
            steps {
                script {
                    bat 'docker-compose up -d'
                }
            }
        }

        stage('Post-Deployment') {
            steps {
                echo 'Deployment complete!'
            }
        }
    }

    post {
        always {
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