pipeline {
  agent any
  stages {
    stage ('test') { // la phase test
            post {
        failure {
          script {
            mail= " test termine avec echec "
          }

        }

        success {
          script {
            mail=" test termine avec succes "
          }

        }

      }
steps {
   bat 'gradle test'
   junit 'build/test-results/test/TEST-Matrix.xml'
          cucumber buildStatus: 'UNSTABLE',
                reportTitle: 'My report',
                fileIncludePattern: 'target/*.json',
                trendsLimit: 10
 
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
          notifyEvents message: 'Signal vous indique que le processus est termine avec succes', token: 'fO811p3_BI6km3eLTE-FdNtE9EY6Sh_F'
        }
    }
}

}
