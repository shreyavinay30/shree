pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "shreyavinay30/shreyaa"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/shreyavinay30/shree.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:v1")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'echo %DOCKER_PASS% | docker login -u shreyavinay30 --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'docker-creds') {
                        docker.image("${DOCKER_IMAGE}:v1").push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Image successfully built and pushed to Docker Hub'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}

