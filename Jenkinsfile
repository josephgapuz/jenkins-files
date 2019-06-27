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
    stage('Run SonarQube') {
      steps {
        bat label: '', script: 'sonar-scanner'
      }
    }
    stage('Deploy Artifact to Archiva') {
      steps {
        bat label: '', script: 'mvn release:prepare release:perform'
        archiveArtifacts(artifacts: 'target/*.war', fingerprint: true)
      }
    } 
    stage('Deploy To Tomcat') {
      steps {
        build job: 'GOT-Deploy-to-Dev'
      }
    }
  }
  
  post {
        always {
            echo 'Sending email notification!'
            
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
            
        }
    }
  
}
