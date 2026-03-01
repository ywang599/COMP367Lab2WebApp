pipeline {
  agent any

  triggers {
    pollSCM('H(30-59)/5 16-23 * 2,3 *')
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build') {
      steps {
        bat 'mvn -B clean package'
      }
      post {
        success {
          archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
        }
      }
    }
  }
}
