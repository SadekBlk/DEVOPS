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
                        sh 'docker build -t react-frontend .'
                    }
                }
            }
        }

        stage('Build Backend') {
            steps {
                script {
                    dir(env.BACKEND_DIR) {
                        sh 'docker build -t node-backend .'
                    }
                }
            }
        }

        stage('Scan for Vulnerabilities') {
            steps {
                script {
                    sh 'trivy image react-frontend'
                    sh 'trivy image node-backend'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'kubectl apply -f k8s/deployment.yaml'
                    sh 'kubectl apply -f k8s/service.yaml'
                }
            }
        }

        stage('Monitor Deployment') {
            steps {
                script {
                    sh 'kubectl get pods -o wide'
                    sh 'kubectl get svc'
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