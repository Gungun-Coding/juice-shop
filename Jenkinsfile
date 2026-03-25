pipeline {
    agent any

    environment {
        AWS_REGION = "eu-north-1"
        ECR_REPO = "juice-shop"
        AWS_ACCOUNT_ID = "376505902368"
        IMAGE_TAG = "latest"
        ECR_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/Gungun-Coding/juice-shop.git'
            }
        }

        
        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${ECR_REPO}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Container Scan') {
            steps {
                sh 'trivy image ${ECR_REPO}:${IMAGE_TAG}'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region ${AWS_REGION} | \
                docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                '''
            }
        }

        stage('Create ECR Repo (if not exists)') {
            steps {
                sh '''
                aws ecr describe-repositories --repository-names ${ECR_REPO} --region ${AWS_REGION} || \
                aws ecr create-repository --repository-name ${ECR_REPO} --region ${AWS_REGION}
                '''
            }
        }

        stage('Tag & Push Image to ECR') {
            steps {
                sh '''
                docker tag ${ECR_REPO}:${IMAGE_TAG} ${ECR_URI}:${IMAGE_TAG}
                docker push ${ECR_URI}:${IMAGE_TAG}
                '''
            }
        }

        stage('Deploy to EKS') {
    steps {
        sh '''
        aws eks update-kubeconfig --region ${AWS_REGION} --name juice-shop-eks

        kubectl set image deployment/juice-shop \
        juice-shop=${ECR_URI}:${IMAGE_TAG}
        '''
    }
}

    }
}