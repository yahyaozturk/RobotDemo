pipeline {
  agent {
    node {
      label 'robot'
    }

  }
  stages {
    stage('Run Robot') {
      steps {
        sh(script: '''apt-get update
apt install python3 python3-pip -y
pip install robotframework --break-system-packages
robot --nostatusrc keyword_driven.robot
rebot -x xunitOut.xml output.xml''', returnStatus: true)
        publishGherkinResults 'xunitOut.xml'
      }
    }

  }
  post {
    always {
      robot(outputPath: '.', passThreshold: 80, unstableThreshold: 70, onlyCritical: false)
    }

  }
}