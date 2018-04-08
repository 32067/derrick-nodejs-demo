pipeline {
   environment {
     REGISTRY_ENDPOINT = "https://192.168.0.31/v2/"
     IMAGE_WITH_TAG = "192.168.0.31/dev/nodejs"
     REGISTRY_CERTS = "registry"
   }
  agent {
    node {
      label 'nodejs'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install --registry=https://registry.npm.taobao.org'
      }
    }
    stage('Test') {
      steps {
        sh 'npm test '
      }
    }
    stage('Code Quality') {
      steps {
        script {
          try{
            checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
          }catch(e){
            echo e
          }
        }

      }
    }
    stage('Image Build&Publish') {
      steps {
        echo 'Build Images'
        script {
          docker.withRegistry("${REGISTRY_ENDPOINT}", "${REGISTRY_CERTS}") {
            sh 'docker build -t ${IMAGE_WITH_TAG} .'
            sh 'docker push ${IMAGE_WITH_TAG}'
          }
        }

      }
    }
  }
  triggers {
    pollSCM('* * * * *')
  }
}