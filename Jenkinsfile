pipeline {
  agent any

  environment {
    REGION = "us-east-1"
    ECR_URI = "017540984476.dkr.ecr.us-east-1.amazonaws.com/jenkins-ecs-app"
  }

  stages {

    stage('Clone') {
      steps {
        git 'https://github.com/Balaji-official-006/Jenkins-eks.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
        docker build -t webapp:$BUILD_NUMBER .
        '''
      }
    }

    stage('Push to ECR') {
      steps {
        sh '''
        aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin $ECR_URI

        docker tag webapp:$BUILD_NUMBER $ECR_URI:$BUILD_NUMBER
        docker push $ECR_URI:$BUILD_NUMBER
        '''
      }
    }

    stage('Deploy to EKS') {
      steps {
        sh '''
        sed -i "s|IMAGE_TAG|$BUILD_NUMBER|g" deployment.yaml
        kubectl apply -f deployment.yaml
        kubectl apply -f service.yaml
        '''
      }
    }
  }
}
