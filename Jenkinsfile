pipeline {

  environment {
    dockerimagename = "ekachnycz/react-app"
    dockerImage = ""
  }

  agent any
  
  stages {

    stage('Checkout Source') {
      steps {
         git branch: 'main', url: 'https://github.com/ekachnycz/jenkins-k8s-deployment.git'
      }
    }
    
    stage('Build') {
        steps{
          script {
            dockerImage = docker.build dockerimagename
          }
        }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        withKubeConfig(credentialsId: 'microk8s') {
          sh 'kubectl apply -f deployment.yaml'
          sh 'kubectl apply -f service.yaml'
        }
      }
    }

  }
