@Library('sharedlibsmmt') _


node('nodes'){
    
def mavenHome = tool name: "maven3.8.4"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

echo "Job name is: ${env.JOB_NAME}" 
echo "Node name is: ${env.NODE_NAME}" 
echo "Build number is: ${env.BUILD_NUMBER}" 


try{

sendSlackNotifications('STARTED')


stage('CheckoutCode')
{
git branch: 'development', credentialsId: 'f5708724-a441-46f4-8f75-3757193ad3f4', url: 'https://github.com/MithunTechnologiesDevOps/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}

stage('TriggerDownstreamJob'){
build job: 'pipelinescriptwithbuildparameters'
}

/*
stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactsIntoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppIntoTomcatServer')
{
sshagent(['f845696b-f9c2-49d9-8648-4fc8252dd8f4']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.12.233:/opt/apache-tomcat-9.0.64/webapps
}
 
}
*/
}catch(e){
currentBuild.result = "FAILED"
    throw e
}
finally{
sendSlackNotifications(currentBuild.result)
}

}//Node Closing

//Belode code will use for send slack build notifications

/*
def sendSlackNotifications(String buildStatus = 'STARTED') {
  
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: 'walmart')
}
*/

