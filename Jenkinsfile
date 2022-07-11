pipeline {

  environment {
    dockerimagename = "enokule/helloworld"
    dockerImage = ""
  }

  agent {
        docker { image 'enokule/jenkins-agent:jenkins-docker' }
    }

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/enokule/java-hello-world.git'
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
