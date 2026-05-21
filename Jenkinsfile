pipeline {
    agent any

    environment {
        IMAGE_NAME = "ggofficial/my-cicd-app"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/ggofficial/cicd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'ggofficial',
                    passwordVariable: 'Syncmaster@773s'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u ggofficial --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop myapp || true
                docker rm myapp || true

                docker pull $IMAGE_NAME:latest

                docker run -d -p 80:80 --name myapp $IMAGE_NAME:latest
                '''
            }
        }
    }
}
