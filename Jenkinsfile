pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/Gungun-Coding/juice-shop.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t juice-shop-app .'
                echo 'Docker image built successfully.'
            }
        }

        stage('Container Scan') {
            steps {
                sh '''
                trivy image --format table -o trivy-report.txt juice-shop-app
                '''
                archiveArtifacts artifacts: 'trivy-report.txt'
            }
        }

        stage('Security Gate') {
            steps {
                sh 'trivy image --exit-code 1 --severity CRITICAL juice-shop-app'
            }
        }

    }
}