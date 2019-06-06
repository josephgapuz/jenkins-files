pipeline {
  agent any
  stages {
    stage('Checkout Project') {
      steps {
        git credentialsId: 'GitHub', url: 'https://github.com/josephgapuz/sandbox.git'
      }
    }
    stage('Maven Compile, JUnit Test & Package') {
      steps {
        sh 'mvn clean install'
        archiveArtifacts(artifacts: 'target/sandbox-1.0-SNAPSHOT.war', fingerprint: true)
      }
    }    
    stage('SonarQube') {
      steps {
        sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:RELEASE:sonar -Dsonar.host.url=http://localhost:9000'
      }
    }   
    stage('Deploy') {
      steps {
        build job: 'GOT-Deploy-to-Dev', parameters: [string(name: 'BRANCH_NAME', value: 'master')]
      }
    }
  }
}
