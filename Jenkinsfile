pipeline {
  agent any
  stages {
    stage('Test') {
      steps {
        bat 'gradlew test'
      }
      
      post {
        always {
            archiveArtifacts artifacts: 'build/reports/tests/**/*.*' //'**/*.jar', fingerprint: true
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
        
        failure {
          notifyEvents message: 'Test failed', token: 'oAl8FaYR4mfbgMb8G-LgjlK064gSemXF'
        }
      }
    }
    
    stage('Code Analysis') {
        steps {
              withSonarQubeEnv('sonar') { 
                bat 'gradlew sonarqube'
              }
        }
      
      post {
          failure {
            notifyEvents message: 'code analysis failed', token: 'oAl8FaYR4mfbgMb8G-LgjlK064gSemXF'
          }
      }
         
    }
    
    stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
      
      post {
            failure {
            notifyEvents message: 'quality gate failed', token: 'oAl8FaYR4mfbgMb8G-LgjlK064gSemXF'
            }
      }
    }
    
    stage('build') {
        steps {
          bat 'gradlew build'
        }
      
        post {
          success {
            bat 'gradlew javadoc'
            archiveArtifacts artifacts:  'build/libs/*.jar,  build/docs/**/*.*', fingerprint: true
          }
          
          failure {
            notifyEvents message: 'build failed', token: 'oAl8FaYR4mfbgMb8G-LgjlK064gSemXF'
          }
        }
    }
    
    
    stage('deploy') {
        steps {
            bat 'gradlew publish'
        }
      
      post {
          failure {
            notifyEvents message: 'deploy failed', token: 'oAl8FaYR4mfbgMb8G-LgjlK064gSemXF'
          }
      }
    }
    
    stage('notification') {
           steps { 
              emailext body: 'build success',
                              subject: 'New build notification',
                              to: 'jm_boumendjel@esi.dz'
             notifyEvents message: 'build success', token: 'oAl8FaYR4mfbgMb8G-LgjlK064gSemXF'
             
           }      
    }
  }

}
