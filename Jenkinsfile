pipeline {

  environment {
    dockerimagename = "ekachnycz/react-app"
    dockerImage = ""
  }


  agent { dockerfile true }
  
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
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }

}
