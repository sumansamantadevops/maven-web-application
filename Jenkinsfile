@Library('sharedlib-slacknotifications') _

node{

def mavenHome = tool name: "maven3.8.4"


properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

sendSlackNotifications('STARTED')    
    
stage('CheckoutCode')
{
git branch: 'development', credentialsId: '4c698820-4fe1-4cbf-a12c-a9c8d797cc4e', url: 'https://github.com/suman-devops-github/maven-web-application.git'
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
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.12.233:/opt/apache-tomcat-9.0.64/webapps"
}

}*/

}
