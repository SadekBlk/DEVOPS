pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'docker build -t react-frontend .'
                }
            }
        }
        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'docker build -t node-backend .'
                }
            }
        }
        stage('Test Frontend') {
            steps {
                dir('frontend') {
                    sh 'docker run react-frontend npm test'
                }
            }
        }
        stage('Test Backend') {
            steps {
                dir('backend') {
                    sh 'docker run node-backend npm test'
                }
            }
        }
        stage('Push Frontend') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    dir('frontend') {
                        sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                        sh 'docker tag react-frontend $DOCKER_USER/react-frontend:latest'
                        sh 'docker push $DOCKER_USER/react-frontend:latest'
                    }
                }
            }
        }
        stage('Push Backend') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    dir('backend') {
                        sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                        sh 'docker tag node-backend $DOCKER_USER/node-backend:latest'
                        sh 'docker push $DOCKER_USER/node-backend:latest'
                    }
                }
            }
        }
    }
}