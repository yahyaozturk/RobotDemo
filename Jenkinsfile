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

stage('Clean Workspace') {
      steps {
        sh 'mvn -Dmaven.test.failure.ignore=true clean'
      }
    }
    stage('Build and Package Microservice') {
      steps {
        sh 'mvn -Dmaven.test.failure.ignore=true build'
      }
    }

  stage('SonarQube analysis') {
      steps {
        addALMOctaneSonarQubeListener pushCoverage: true, pushVulnerabilities:true, sonarToken:env.SONAR_AUTH_TOKEN, sonarServerUrl:env.SONAR_HOST_URL
        script {
          scannerHome = tool 'SONAR'
        }

        withSonarQubeEnv('SONAR') {
          sh "${scannerHome}/bin/sonar-scanner"
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