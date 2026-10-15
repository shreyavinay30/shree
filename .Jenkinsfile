pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "shreyavinay30/shreyaa"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/shreyavinay30/shree.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:v1 .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(
                    credentialsId: 'dockerhub-PAT',
                    variable: 'DOCKER_PAT'
                )]) {
                    bat '''
                    @echo off
                    echo %DOCKER_PAT%|docker login -u shreyavinay30 --password-stdin
                   if errorlevel 1 exit /b 1
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %DOCKER_IMAGE%:v1'
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
