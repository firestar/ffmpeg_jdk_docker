pipeline {
  agent any
  environment {
    DOCKER_ACCOUNT = 'firestarthehack'
    IMAGE_VERSION = '1.01'
    IMAGE_NAME = 'ffmpeg-jdk'
    RANCHER_STACK_NAME = 'AnimeCap'
    RANCHER_SERVICE_NAME = 'FFMPEG_SERVER'
    RANCHER_SERVICE_URL = 'http://34.215.0.188:8080/v2-beta'
  }
  stages {
    stage('Docker Build') {
      steps {
        parallel(
          "Build Docker Image": {
            sh "docker build -t ${env.DOCKER_ACCOUNT}/${env.IMAGE_NAME}:${env.IMAGE_VERSION} ./"
          }
        )
      }
    }
    stage('Publish Latest Image') {
      steps {
        sh "docker push ${env.DOCKER_ACCOUNT}/${env.IMAGE_NAME}:${env.IMAGE_VERSION}"
      }
    }
    stage('Deploy') {
      steps {
        rancher(environmentId: '1a5', ports:'', environments:'', confirm: true, image: "${env.DOCKER_ACCOUNT}/${env.IMAGE_NAME}:${env.IMAGE_VERSION}", service: "${env.RANCHER_STACK_NAME}/${env.RANCHER_SERVICE_NAME}", endpoint: "${env.RANCHER_SERVICE_URL}", credentialId: 'rancher-server')
      }
    }
  }
}
