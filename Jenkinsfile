pipeline {
  agent any
  stages {
    stage('Checkout Project') {
      steps {
        git credentialsId: 'GitHub', url: 'https://github.com/josephgapuz/sandbox.git'
      }
    }
    stage('Build-Test') {
      steps {
        bat label: '', script: 'mvn clean install'
        archiveArtifacts(artifacts: 'target/sandbox-1.0-SNAPSHOT.war', fingerprint: true)
      }
    }    
    stage('SonarQube') {
      steps {
        bat label: '', script: 'sonar-scanner'
      }
    }   
    stage('Deploy') {
      steps {
        build job: 'GOT-Deploy-to-Dev', parameters: [string(name: 'BRANCH_NAME', value: 'master')]
      }
    }
  }
}
