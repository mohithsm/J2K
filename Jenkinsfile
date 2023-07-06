pipeline {

  environment {
        registry = "dockerqprofiles/j2k:1.0.0"
        registryCredential = 'DockerHubCreds' 
  }

  agent {
    node {
        label 'Built-In Node'
    }
  }

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/mohithsm/J2K.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'DockerHubCreds'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          sh "kubectl apply -f ./deployment.yaml -n qp"
          sh "kubectl apply -f ./service.yaml -n qp"
        }
      }
    }

  }
}
