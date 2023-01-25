pipeline {
  agent any
  stages {
   
   stage ('Test') { // la phase build
steps {
bat 'gradlew test'
 junit 'build/test-results/test/TEST-Matrix.xml'
 
   cucumber buildStatus: 'UNSTABLE',
                reportTitle: 'My report',
                fileIncludePattern: 'target/report.json',
     
                trendsLimit: 10

}
}
   
   
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              bat 'gradle sonar'
            }


          }
        }
   
   
         stage("Code Quality") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }
   
   
     stage('build') {
     
   
      steps {
        bat(script: 'gradle build', label: 'gradle build')
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        junit(testResults: 'build/reports/tests/test', allowEmptyResults: true)
       
      }
    }
   
     stage('Deploy') {
      steps {
        bat 'gradle publish'
      }
    }
   
      stage(' deploy Mail Notification') {
      steps {
        mail(subject: 'TPOGL Jenkins notification', body: mail, cc: 'js_rouibi@esi.dz' ,bcc:'js_rouibi@esi.dz')
      }
    }
      stage('Slack Notification') {
      steps {
        slackSend(message: 'Slack vous indique que le processus est termine avec succes. ')
      }
      }
        stage('Signal notification'){
          steps{
          notifyEvents message: 'Signal vous indique que le processus est termine avec succes', token: 'tGffxCY2W0dLrytKTLs9Y02pAeVqamkj'
        }
    }
   
   
   
   
   
}
 

 }
