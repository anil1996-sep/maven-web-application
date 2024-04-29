node{

def mavenHome = tool name: 'maven3.9.5'

echo "The Job name is : ${env.JOB_NAME}"
echo "Jenkins Home directory is : ${env.JENKINS_HOME}"
echo "The Jenkins node name is : ${env.NODE_NAME}"
echo "The Build number is : ${env.BUILD_NUMBER}"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckoutCode'){
git branch: 'development', credentialsId: '1d940131-4ce8-41d3-a885-d6a23e1f18fe', url: 'https://github.com/anil1996-sep/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcat'){
sshagent(['052e002f-4af3-425b-bde0-2e9083614a50']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.32.10:/opt/apache-tomcat-9.0.86/webapps/"

}
}

}
