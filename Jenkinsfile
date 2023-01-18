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
    
    stage('Code Analysis') {
        steps {
              withSonarQubeEnv('sonar') { 
                bat 'gradlew sonarqube'
              }
        }
         
    }
    
    stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
    }
    
    stage('build') {
        steps {
          bat 'gradlew build'
        }
      
        post {
          success {
            bat 'gradlew javadoc'
          }
        }
    }
  }

}
