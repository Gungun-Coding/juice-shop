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

    }
}