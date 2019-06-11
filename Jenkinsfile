pipeline {
  agent any
  stages {
    stage('Checkout Project') {
      steps {
        git credentialsId: 'GitHub', url: 'https://github.com/josephgapuz/sandbox.git'
      }
    }
    stage('Build-Test-Deploy') {
      steps {
        bat label: '', script: 'mvn clean install deploy'
        archiveArtifacts(artifacts: 'target/sandbox-1.0-SNAPSHOT.war', fingerprint: true)
      }
    }    
    stage('SonarQube') {
      steps {
        bat label: '', script: 'sonar-scanner'
      }
    }     
    stage('Deploy To Tomcat') {
      steps {
        build job: 'GOT-Deploy-to-Dev'
      }
    }
  }
}
