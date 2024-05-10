pipeline {
  agent {
    node {
      label 'robot'
    }

  }
  stages {
    stage('Run Robot') {
      steps {
        sh(script: '''apt install python3 -y
pip install robotframework
robot --nostatusrc my_tests.robot''', returnStatus: true)
      }
    }

  }
  post {
    always {
      robot(outputPath: '.', passThreshold: 80, unstableThreshold: 70, onlyCritical: false)
    }

  }
}