pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'oliquavious/my-web-app'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy to Local Docker Host') {
            steps {
                sh '''
                    docker rm -f my-web-app || true
                    docker run -d --name my-web-app -p 8081:80 oliquavious/my-web-app:latest
                '''
            }
        }

        stage('Deploy to Remote Docker Host') {
            steps {
                sshagent(['remote-host-ssh-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no user@remote-host "docker pull oliquavious/my-web-app:latest"
                        ssh -o StrictHostKeyChecking=no user@remote-host "docker rm -f my-web-app || true"
                        ssh -o StrictHostKeyChecking=no user@remote-host "docker run -d --name my-web-app -p 8080:80 oliquavious/my-web-app:latest"
                    '''
                }
            }
        }
    }
}
