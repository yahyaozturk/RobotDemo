pipeline {
  agent {
    node {
      label 'robot'
    }

  }
  stages {
    stage('Run Robot') {
      steps {
        sh(script: '''printenv
pwd
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