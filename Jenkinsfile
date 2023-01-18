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
                reportTitle: 'My report',
                fileIncludePattern: '**/*.json',
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
  }

}
