pipeline {
  agent { label 'master' }

  environment {
    REGION = "us-east-1"
    ECR_URI = "017540984476.dkr.ecr.us-east-1.amazonaws.com/jenkins-ecs-app"
  }

  stages {

    stage('Build Docker Image') {
      steps {
        sh '''
        echo "Building Docker image with tag $BUILD_NUMBER"
        docker build -t webapp:$BUILD_NUMBER .
        '''
      }
    }

    stage('Login to ECR & Push Image') {
      steps {
        sh '''
        echo "Logging into ECR"
        aws ecr get-login-password --region $REGION | docker login --username AWS --password-stdin $ECR_URI

        echo "Tagging image"
        docker tag webapp:$BUILD_NUMBER $ECR_URI:$BUILD_NUMBER

        echo "Pushing image to ECR"
        docker push $ECR_URI:$BUILD_NUMBER
        '''
      }
    }

    stage('Deploy to EKS') {
      steps {
        sh '''
        echo "Updating deployment.yaml with build number"
        sed -i "s|IMAGE_TAG|$BUILD_NUMBER|g" deployment.yaml

        echo "Deploying to EKS"
        kubectl apply -f deployment.yaml
        kubectl apply -f service.yaml

        echo "Deployment completed"
        '''
      }
    }
  }

  post {
    success {
      echo "Pipeline completed successfully 🚀"
    }
    failure {
      echo "Pipeline failed ❌"
    }
  }
}
