pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="218463550458"
        AWS_DEFAULT_REGION="ap-south-1"
        IMAGE_REPO_NAME="react-app"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "218463550458.dkr.ecr.ap-south-1.amazonaws.com/react-app"
    }
   
    stages {
        
         stage('Logging into AWS ECR') {
            steps {
                script {
                sh """aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"""
                }
                 
            }
        }
        
        stage('Cloning Git repo') {
            steps {
                git branch: 'main', url: 'https://github.com/Murshidtp/Push-Docker-Image-to-ECR-usnig-Jenkins.git'   
            }
        }
    stage('Building Docker image') {
      steps{
        script {
          sh "docker build -t ${IMAGE_REPO_NAME} ."
        }
      }
    }
    stage('Pushing to ECR') {
     steps{  
         script {
                sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
         }
        }
      }
   
    }
}
