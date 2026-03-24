pipeline {
    agent any

    environment {
        IMAGE_NAME = "juice-shop-app"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/Gungun-Coding/juice-shop.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Trivy Scan') {
            steps {
                sh 'trivy image --severity HIGH,CRITICAL $IMAGE_NAME'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop juice || true
                docker rm juice || true
                docker run -d -p 3000:3000 --name juice $IMAGE_NAME
                '''
            }
        }
    }
}