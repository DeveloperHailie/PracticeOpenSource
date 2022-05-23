pipeline {
  environment {
    registry = "pjbear/test"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Clone repository') {
      steps { 
        git 'https://github.com/jwpark-sungshin/fork_vs_vfork.git'
      }
    }
    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build("${registry}:${env.BUILD_NUMBER}")
        }
      }
    }
    stage('Test image') {
      steps {
        script {
          dockerImage.inside { 
            sh 'make test' 
          }
        }
      }
    }
    stage('Push image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
            dockerImage.push("${env.BUILD_NUMBER}")
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploy image') {
      when { branch 'master' }
      steps {
        echo "Deploy image ${registry}:${env.BUILD_NUMBER}"
      }
    }
  }
}
