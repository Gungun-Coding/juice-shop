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
        withSonarQubeEnv('sonarqube') {
            sh '''
            sonar-scanner \
              -Dsonar.projectKey=Juice-shop1 \
              -Dsonar.sources=. \
              -Dsonar.host.url=http://ec2-16-171-27-169.eu-north-1.compute.amazonaws.com:9000 \
              -Dsonar.login='sqp_636136e21f606c7547b17a21d188caaa9bd69ee8'
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