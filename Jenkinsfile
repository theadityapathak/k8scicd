pipeline {

  environment {
    registry = "18.144.87.142:8123/nobody/web"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/theadityapathak/k8scicd.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry('http://18.144.87.142:8123' , 'docker-private-credentials' ) {
            sh "docker login http://18.144.87.142:8123"
            dockerImage.push("${env.BUILD_NUMBER}")
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
