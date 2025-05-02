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
          dockerImage = bat(script: "docker build -t ${DOCKER_IMAGE} .", returnStdout: true).trim()
        }
      }
    }

    stage('Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'ebf25043-743c-4360-9ed5-5afff8fb1095', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          script {
            bat "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
            bat "docker push ${DOCKER_IMAGE}"
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
