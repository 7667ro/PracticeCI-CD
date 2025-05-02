pipeline {
  agent any

  environment {
    DOCKER_IMAGE = '12roshan/backend'
  }

  stages {
    stage('Clone Code') {
      steps {
        git credentialsId: 'git', url: 'https://github.com/7667ro/PracticeCI-CD.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          dockerImage = docker.build("${DOCKER_IMAGE}")
        }
      }
    }

    stage('Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'ebf25043-743c-4360-9ed5-5afff8fb1095', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          script {
            sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
            sh "docker push ${DOCKER_IMAGE}"
          }
        }
      }
    }
  }

  post {
    success {
      echo '✅ Successfully pushed to Docker Hub!'
    }
    failure {
      echo '❌ Build failed.'
    }
  }
}
