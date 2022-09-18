pipeline {

  environment {
    dockerimagename = "nikhils9/ip-app:latest"
    dockerImage = ""
  }

  agent { label 'master' }

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/Nikh-S9/CI-CD.git'
      }
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
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
