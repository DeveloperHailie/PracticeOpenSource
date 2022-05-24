pipeline {
  environment {
    registry = "hailiedev/test"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Clone repository') { 
      steps { 
        git 'https://github.com/DeveloperHailie/PracticeOpenSource.git'
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
            # 업로드할 레지스트리 정보, Jenkins Credentials ID
            dockerImage.push("${env.BUILD_NUMBER}") # 이미지에 빌드번호를 태그로 붙인 후 push
            dockerImage.push("latest") # 이미지에 latest를 태그로 붙인 후 push
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
