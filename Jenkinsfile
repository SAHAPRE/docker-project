pipeline {
    agent any 
    environment {
        // AWS ECR details
        AWS_REGION = 'ap-south-1'
        AWS_ACCOUNT_ID = '861276111806'
        ECR_REPO_NAME = 'prod/my-project'
        //IMAGE_TAG = "latest
        //IMAGE_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}"
        
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from your Git repository
                git 'https://github.com/SAHAPRE/docker-project.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t prod/my-project .'
                }
            }
        }
        stage('Login to AWS ECR') {
            steps {
                script {
                    // Login to AWS ECR
                    sh '$(aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 861276111806.dkr.ecr.ap-south-1.amazonaws.com)'
                }
            }
        }

        stage('Push Docker Image to AWS ECR') {
            steps {
                script {
                    // Push the Docker image to AWS ECR
                   sh "docker tag prod/my-project:latest 861276111806.dkr.ecr.ap-south-1.amazonaws.com/prod/my-project:v12"
                  sh "docker push $IMAGE_URI"
                }
            }
        }
        
    post {
        success {
            echo 'Pipeline successfully completed!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
