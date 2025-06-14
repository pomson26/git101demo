pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID = "982544208576"
        AWS_DEFAULT_REGION = "us-east-1" 
        IMAGE_REPO_NAME = "devopsint"
        IMAGE_TAG = "latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
   
    stages {
        stage('Logging into AWS ECR') {
            steps {
                script {
                    sh """
                        aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | \
                        docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com
                    """
                }
            }
        }
        
        stage('Cloning Git') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[
                        credentialsId: '33c8457e-369a-4879-8303-33b925ae65d2',
                        url: 'https://github.com/pomson26/git101demo.git'
                    ]]
                )     
            }
        }
  
        stage('Building image') {
            steps {
                script {
                    sh 'docker buildx create --use --name jenkins-builder || true'
                    sh """
                        docker buildx build \
                        -t ${REPOSITORY_URI}:${IMAGE_TAG} \
                        --platform linux/amd64 \
                        --load .
                    """
                }
            }
        }
   
        stage('Pushing to ECR') {
            steps {  
                script {
                    sh "docker push ${REPOSITORY_URI}:${IMAGE_TAG}"
                }
            }
        }
    }
}
