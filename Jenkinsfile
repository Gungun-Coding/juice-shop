pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/Gungun-Coding/juice-shop.git'
            }
        }

        stage('SonarQube Scan') {
    steps {
        withSonarQubeEnv('SonarQube') {
            sh '''
            sonar-scanner \
            -Dsonar.projectKey=juice-shop \
            -Dsonar.sources=.
            '''
        }
    }
}

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t juice-shop-app .'
            }
        }

        stage('Container Scan') {
            steps {
                sh 'trivy image juice-shop-app'
            }
        }

    }
}