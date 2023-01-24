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
              bat 'gradlew sonar'
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
        bat(script: 'gradlew build', label: 'gradlew build')
        bat 'gradle javadoc'
        archiveArtifacts 'build/libs/*.jar'
        junit(testResults: 'build/reports/tests/test', allowEmptyResults: true)
       
      }
    }
   
    
    
    
     stage('Deploy') {
      steps {
        bat 'gradlew publish'
      }
    }
   
    
    
    
     stage('Notification') {
      steps {
       
       notifyEvents message: 'New build is Created success', token: 'tGffxCY2W0dLrytKTLs9Y02pAeVqamkj'
         notifyEvents message: 'New build field', token: 'tGffxCY2W0dLrytKTLs9Y02pAeVqamkj'

        slackSend(baseUrl: 'https://hooks.slack.com/services/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', message: 'New build is Created', channel: '#tps')
      }
    }
 
}
 
  post {
        success {
          mail(subject: 'Build Success', body: 'New Build is deployed !', from: 'js_rouibi@esi.dz', to: 'js_rouibi@esi.dz')
        }
        failure {
          mail(subject: 'Build Failure', body: "the new build isn't deployed succesfully !", from: 'js_rouibi@esi.dz', to: 'js_rouibi@esi.dz')
        }
       
      }

 }
