pipeline {
  agent {
    node {
      label 'robot'
    }

  }
  stages {
    stage('Run Robot') {
      steps {
        sh 'robot --nostatusrc keyword_driven.robot'
      }
    }

  }
}