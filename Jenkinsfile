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

        stage('Scan for Vulnerabilities') {
            steps {
                script {
                    bat 'trivy image react-frontend'
                    bat 'trivy image node-backend'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    bat 'kubectl apply -f k8s/deployment.yaml'
                    bat 'kubectl apply -f k8s/service.yaml'
                }
            }
        }

        stage('Monitor Deployment') {
            steps {
                script {
                    bat 'kubectl get pods -o wide'
                    bat 'kubectl get svc'
                }
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