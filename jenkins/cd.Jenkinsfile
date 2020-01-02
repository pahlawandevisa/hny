pipeline {
  agent {
    label 'docker'
  }

  environment {
    CI = 'true'
    IMAGE_NAME = 'melchor9000/hny'
  }

  stages {
    stage('Build image') {
      steps {
        docker.build(IMAGE_NAME, '--pull -f docker/build.Dockerfile .')
      }
    }

    stage('Push image') {
      steps {
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-melchor9000') {
            def image = docker.image(IMAGE_NAME)
            image.push('latest')
          }
        }
      }
    }

    stage('Deploy') {
      steps {
        withCredentials([string(credentialsId: 'deploy-webhook-url', variable: 'WEBHOOK_URL')]) {
          sh 'curl -X POST $WEBHOOK_URL'
        }
      }
    }
  }
}
