pipeline {
agent any

environment {
    AWS_REGION     = "ap-south-1"
    AWS_ACCOUNT_ID = "933747314862"
    ECR_REPO       = "myapp"
    IMAGE_TAG      = "${BUILD_NUMBER}"
}

stages {

    stage('Checkout') {
        steps {
            git branch: 'main',
                credentialsId: 'githubcredentials',
                url: 'https://github.com/prajwal-rijo/my-app.git'
        }
    }

    stage('SonarQube Analysis') {
        steps {
            echo "Running SonarQube Analysis"
        }
    }

    stage('Build Docker Image') {
        steps {
            sh '''
            docker build -t ${ECR_REPO}:${IMAGE_TAG} .
            '''
        }
    }

    stage('Push Image to ECR') {
        steps {
            sh '''
            aws ecr get-login-password --region ${AWS_REGION} | \
            docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com

            docker tag ${ECR_REPO}:${IMAGE_TAG} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:${IMAGE_TAG}

            docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:${IMAGE_TAG}
            '''
        }
    }
}

post {
    success {
        echo "Image pushed successfully to ECR"
    }

    failure {
        echo "Pipeline failed"
    }
}
}
}

