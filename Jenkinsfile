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
    
    stage('Initialize'){
           def dockerHome = tool 'myDocker'
           env.PATH = "${dockerHome}/bin:${env.PATH}"
       }
    
    stage('Build image') {
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
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }

}
