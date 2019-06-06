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
        sh 'mvn clean install'
        archiveArtifacts(artifacts: 'target/sandbox-1.0-SNAPSHOT.war', fingerprint: true)
      }
    }    
    stage('SonarQube') {
      steps {
        sh 'sonar-scanner'
      }
    }   
    stage('Deploy') {
      steps {
        build job: 'GOT-Deploy-to-Dev', parameters: [string(name: 'BRANCH_NAME', value: 'master')]
      }
    }
  }
}
