pipeline {
  agent any
  stages {
    stage('Test') {
      steps {
        bat 'gradlew test'
      }
      
      post {
        always {
            archiveArtifacts artifacts: '**/*.jar', fingerprint: true
            junit 'build/reports/**/*.xml'
        }
      }
    }
  }

}
