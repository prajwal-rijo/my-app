pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        AWS_ACCOUNT_ID = "YOUR_ACCOUNT_ID"
        ECR_REPO = "myapp"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/USERNAME/myapp.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube Analysis"
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t myapp:${BUILD_NUMBER} .'
            }
        }

        stage('Push to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.ap-south-1.amazonaws.com

                docker tag myapp:${BUILD_NUMBER} ${AWS_ACCOUNT_ID}.dkr.ecr.ap-south-1.amazonaws.com/myapp:${BUILD_NUMBER}

                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.ap-south-1.amazonaws.com/myapp:${BUILD_NUMBER}
                '''
            }
        }
    }
}
