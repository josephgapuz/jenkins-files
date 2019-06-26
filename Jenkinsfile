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
        junit allowEmptyResults: true, testResults: '**target/surefire-reports/*.xml'
      }
    }    
    stage('SonarQube') {
      steps {
        bat label: '', script: 'sonar-scanner'
      }
    }
    stage('Deploy') {
      steps {
        bat label: '', script: 'mvn release:prepare release:perform -Darguments="-Dmaven.test.skip=true"'
        archiveArtifacts(artifacts: 'target/*.war', fingerprint: true)
      }
    } 
    stage('Deploy To Tomcat') {
      steps {
        build job: 'GOT-Deploy-to-Dev'
      }
    }
  }
}
