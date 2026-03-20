pipeline {
  agent any

  environment {
    IMAGE_NAME = "ywang599/comp367-webapp"
    IMAGE_TAG  = "latest"
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build Maven Project') {
      steps {
        // Build WAR so Dockerfile can COPY it from target/
        bat 'mvnw.cmd -B clean package'
      }
    }

    stage('Docker Login'){
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
        bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
        }
      }
    }

    stage('Docker Build'){
      steps {
        bat 'docker build -t %IMAGE_NAME%:%IMAGE_TAG% .'
      }
    }

    stage('Docker Push') {
      steps {
        bat 'docker push %IMAGE_NAME%:%IMAGE_TAG%'
      }
    }
  }
}
