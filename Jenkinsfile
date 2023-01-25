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
   
   
    
    
     

 }
