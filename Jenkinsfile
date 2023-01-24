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
   bat 'gradlew test'
   junit 'build/test-results/test/TEST-Matrix.xml'
          cucumber buildStatus: 'UNSTABLE',
                reportTitle: 'My report',
                fileIncludePattern: 'target/*.json',
                trendsLimit: 10
 
}
    }
    
    stage('test Mail Notification') {
      steps {
        mail(subject: 'TPOGL Jenkins test phase notification', body: mail, cc: 'js_rouibi@esi.dz' ,bcc:'js_rouibi@esi.dz')
      }
    }
   
     
        stage('Code Analysis') {
                post {
        failure {
          script {
            mail= " Code Analysis termine avec echec "
          }

        }

        success {
          script {
            mail=" Code analysis termine avec succes "
          }

        }

      }
         
          steps {
            withSonarQubeEnv('sonar') {
              bat 'gradle sonar'
            }

            waitForQualityGate true
          }
        }
    
    
    
   
}

}
