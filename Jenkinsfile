pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub'
        DOCKER_IMAGE_FRONTEND = 'react-frontend'
        DOCKER_IMAGE_BACKEND = 'node-backend'
        GITHUB_CREDENTIALS_ID = 'github-credentials' // Use the ID you created
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']], // Change 'main' to your branch name if different
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/SadekBlk/DEVOPS.git', // Replace with your GitHub repo URL
                        credentialsId: "${GITHUB_CREDENTIALS_ID}"
                    ]]
                ])
            }
        }
        stage('Build Frontend') {
            steps {
                dir('Frontend') {
                    sh 'docker build -t $DOCKER_IMAGE_FRONTEND .'
                }
            }
        }
        stage('Build Backend') {
            steps {
                dir('Backend') {
                    sh 'docker build -t $DOCKER_IMAGE_BACKEND .'
                }
            }
        }
        stage('Scan Frontend') {
            steps {
                sh 'trivy image $DOCKER_IMAGE_FRONTEND'
            }
        }
        stage('Scan Backend') {
            steps {
                sh 'trivy image $DOCKER_IMAGE_BACKEND'
            }
        }
        stage('Push Frontend') {
            steps {
                withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                    sh 'docker tag $DOCKER_IMAGE_FRONTEND $DOCKER_USER/$DOCKER_IMAGE_FRONTEND:latest'
                    sh 'docker push $DOCKER_USER/$DOCKER_IMAGE_FRONTEND:latest'
                }
            }
        }
        stage('Push Backend') {
            steps {
                withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                    sh 'docker tag $DOCKER_IMAGE_BACKEND $DOCKER_USER/$DOCKER_IMAGE_BACKEND:latest'
                    sh 'docker push $DOCKER_USER/$DOCKER_IMAGE_BACKEND:latest'
                }
            }
        }
    }
}