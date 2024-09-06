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
      agent {
          docker {
              image 'gradle:8.2.0-jdk17-alpine'
              // Run the container on the node specified at the
              // top-level of the Pipeline, in the same workspace,
              // rather than on a new node entirely:
              reuseNode true
                }
            }
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
