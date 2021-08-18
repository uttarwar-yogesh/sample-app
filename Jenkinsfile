pipeline {
  environment {
    imagename = "sample-app"
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
    stage('Build Docker image') {
      steps{
        script {
           dockerImage = docker.build("pdmdevopsdemo:${env.BUILD_NUMBER}")
        }
      }
    }
    stage('Publish Docker Image to Nexus') {
      steps{
        script {
          docker.withRegistry('pdmdev.azurecr.io') {
           dockerImage.push("$BUILD_NUMBER")
           dockerImage.push('latest')
          }
        }
      }
    }
    stage('Remove Unused Docker image') {
      steps{
        sh '''
           docker rmi $imagename:$BUILD_NUMBER
        '''   
      }
    }
  }
}
