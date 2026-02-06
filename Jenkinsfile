pipeline {
  agent any

  environment {
    IMAGE_NAME = "chigullanaveen/eks-demo-app"
    IMAGE_TAG = "latest"
  }

  stages {

    stage('Checkout') {
      steps {
        git 'https://github.com/chigullanaveen-cell/eks-demo-app.git'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
      }
    }

    stage('Docker Login') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'USER',
          passwordVariable: 'PASS'
        )]) {
          sh 'echo $PASS | docker login -u $USER --password-stdin'
        }
      }
    }

    stage('Docker Push') {
      steps {
        sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
      }
    }

    stage('Deploy to EKS') {
      steps {
        sh 'kubectl apply -f k8s-deploy.yaml'
      }
    }

  }
}
