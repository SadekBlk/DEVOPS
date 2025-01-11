pipeline {
    agent any
    stages {
        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'docker build -t my-frontend .'
                }
            }
        }
        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'docker build -t my-backend .'
                }
            }
        }
        stage('Test Frontend') {
            steps {
                dir('frontend') {
                    sh 'docker run my-frontend npm test'
                }
            }
        }
        stage('Test Backend') {
            steps {
                dir('backend') {
                    sh 'docker run my-backend npm test'
                }
            }
        }
        stage('Push Frontend') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    dir('frontend') {
                        sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                        sh 'docker tag my-frontend $DOCKER_USER/my-frontend:latest'
                        sh 'docker push $DOCKER_USER/my-frontend:latest'
                    }
                }
            }
        }
        stage('Push Backend') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    dir('backend') {
                        sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                        sh 'docker tag my-backend $DOCKER_USER/my-backend:latest'
                        sh 'docker push $DOCKER_USER/my-backend:latest'
                    }
                }
            }
        }
    }
}