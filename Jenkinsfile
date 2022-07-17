node
{

def mavenHome = tool name: "maven-3.8.4"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckoutCode')
{
git branch: 'development', credentialsId: '019785f1-6691-47d8-beb5-8c1bed2778a1', url: 'https://github.com/suman-devops-github/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactsintoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppintoTomcatServer')
{
sshagent(['6855f365-7f0f-42ab-9f2f-6de5bd837380']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.2.42:/opt/apache-tomcat-9.0.64/webapps"  
}
}

}


