pipeline {
  agent any
  stages {
    stage('Checkout Project') {
      steps {
        git credentialsId: 'GitHub', url: 'https://github.com/josephgapuz/jenkins-files.git'
      }
    }
    stage('Compile') {
      steps {
        sh 'mvn compile -Dmaven.test.skip=true'
      }
    }
    stage('JUnit Test') {
      steps {
        sh 'mvn test'
      }
    }
    stage('SonarQube') {
      steps {
        sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:RELEASE:sonar -Dsonar.host.url=http://localhost:9000'
      }
    }
    stage('Package') {
      steps {
        sh 'mvn package -Dmaven.test.skip=true'
        archiveArtifacts(artifacts: 'target/sandbox-1.0-SNAPSHOT.war', fingerprint: true)
      }
    }
    stage('Deploy') {
      steps {
        build '../GOT-Deploy-to-Dev'
      }
    }
  }
}
