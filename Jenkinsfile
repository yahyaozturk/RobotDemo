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
apt install python3 python3-pip maven -y
pip install robotframework --break-system-packages
robot --nostatusrc keyword_driven.robot
rebot -x xunitOut.xml output.xml''', returnStatus: true)
      }
    }

    stage('Clean Workspace') {
      steps {
        sh 'mvn -Dmaven.test.failure.ignore=true clean'
      }
    }

    stage('Build and Package Microservice') {
      steps {
        sh 'mvn -Dmaven.test.failure.ignore=true compile'
      }
    }

    stage('SonarQube analysis') {
      steps {
        script {
          scannerHome = tool 'SONAR'
        }

        withSonarQubeEnv('SONAR') {
          sh "${scannerHome}/bin/sonar-scanner"
        }

        addALMOctaneSonarQubeListener(sonarToken: 'squ_23dc266d33e16841584e5ba756d674b2874ca688', sonarServerUrl: 'http://host.docker.internal:9000', pushCoverage: true, pushVulnerabilities: true)
      }
    }

  }
  post {
    always {
      junit '*.xml'
    }

  }
}