pipeline {
  environment {
    imagename = "pdmdevopsdemo"
    dockerImage = ''
  }
  agent { label 'master' }
  
  stages {
    stage('Build Springboot App') {
      steps {
        sh "mvn clean install"
      }
    }
    stage('Publish Maven artifact to nexus') {
      steps {
        sh "mvn clean deploy -Dmaven.test.skip=true"
      }
    }
  }
}
