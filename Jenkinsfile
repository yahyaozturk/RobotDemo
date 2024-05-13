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
      }
    }

  


  stage('Git Checkout') {
      steps {
          checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/yahyaozturk/example-spring-app']])
          echo 'Git Checkout Completed'
      }
  }

  stage('SonarQube Analysis') {
      steps {
          withSonarQubeEnv('ServerNameSonar') {
              sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=example-spring-app -Dsonar.projectName="example-spring-app"'
              echo 'SonarQube Analysis Completed'
          }
      }
  }
 }

  post {
    always {
      junit '*.xml'
    }

  }
}