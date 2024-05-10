pipeline {
  agent {
    node {
      label 'robot'
    }

  }
  stages {
    stage('Run Robot') {
      steps {
        sh(script: 'pip3 install robotframework --break-system-packages', returnStatus: true)
        sh(script: 'robot --nostatusrc my_tests.robot', returnStatus: true)
      }
    }

  }
  post {
    always {
      robot(outputPath: '.', passThreshold: 80, unstableThreshold: 70, onlyCritical: false)
    }

  }
}