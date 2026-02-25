pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "meghashree1227/onlinebookstore"
        DOCKER_CREDS = "dockerhub-creds"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/shreemegha1227-tech/onlinebookstore.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDS}",
                usernameVariable: 'USER',
                passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                }
            }
        }

        stage('Push Image') {
            steps {
                sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
            }
        }

        stage('Deploy Container') {
            steps {
                sh """
                docker stop bookstore || true
                docker rm bookstore || true
                docker run -d -p 80:8080 --name bookstore ${DOCKER_IMAGE}:${BUILD_NUMBER}
                """
            }
        }
    }
}
