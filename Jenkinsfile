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
            junit '**/*.xml'
          
            cucumber buildStatus: 'UNSTABLE',
                reportTitle: 'cucumber_report',
                fileIncludePattern: 'target/report.json',
                trendsLimit: 10,
                classifications: [
                    [
                        'key': 'Browser',
                        'value': 'Chrome'
                    ]
                ]
        }
      }
    }
    
    stage('Code analysis') {
        steps {
              withSonarQubeEnv('sonar') { 
                bat 'gradlew sonarqube'
              }
        }
         
    }
  }

}
