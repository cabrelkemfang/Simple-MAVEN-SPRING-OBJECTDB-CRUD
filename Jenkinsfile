node {
    def branchName = "${env.BRANCH_NAME}"
     echo "${branchName}"
    try {
        stage('Slack start notification'){
            notifyBuild('STARTED')
        }
        
         stage('Clone the project') {
             git branch: 'master', url: 'https://github.com/cabrelkemfang/Simple-MAVEN-SPRING-OBJECTDB-CRUD.git'
        }
  
        stage('Compiling, Test ,Packaging') {
          sh label: '', script: 'mvn clean package'
        }
  
       stage('Sending Email'){
         mail bcc: '', body: 'Hello Is jenkins', cc: '', from: '', replyTo: '', subject: 'Jenkking test', to: 'ghislaincabrel.kemfang@gmail.com'
       }
  
       stage('Archive') {
       
         step([$class: 'JUnitResultArchiver', testResults: 'target/surefire-reports/TEST-*.xml'])
     
       archiveArtifacts 'target/*.war'
       }
  
stage('deploying'){
deploy adapters: [tomcat8(credentialsId: 'Tomcat user', path: '', url: 'http://localhost:8088/')], contextPath: 'springtest', war: ' **/*.war'
}
    } catch (e) {
         currentBuild.result = "FAILED"
    throw e
    } finally {
        stage('Slack notification'){
             // Success or failure, always send notifications
             notifyBuild(currentBuild.result)
        }
   
  }
 
  
//   stage("Slack Notification"){
//       slackSend baseUrl: 'https://hooks.slack.com/services/', 
//       channel: 'deployment', color: 'good',
//       message: 'Hello Jenkins', 
//       teamDomain: 'https://sevencommonfa-7sr9737', 
//       tokenCredentialId: 'SLack-deployment', 
//       username: 'cabrelkemfang'
//   }

 
}

def notifyBuild(String buildStatus = 'STARTED') {
    
    def branchName = "${env.BRANCH_NAME}"
    
  // build status of null means successful
  buildStatus = buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' Branch : '${env.BRANCH_NAME}'"
  def summary = "${subject} (${env.BUILD_URL})"
  
  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
  } else {
    color = 'RED'
  }
  
   // Send notifications
 slackSend baseUrl: 'https://hooks.slack.com/services/', 
      channel: 'deployment', color: color,
      message: summary, 
      teamDomain: 'https://sevencommonfa-7sr9737', 
      tokenCredentialId: 'SLack-deployment', 
      username: 'cabrelkemfang'
}
