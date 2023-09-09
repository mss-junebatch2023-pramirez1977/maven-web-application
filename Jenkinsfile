node{
    
echo "Jenkins Home dir is: ${env.JENKINS_HOME}"
echo "Job name is: ${env.JOB_NAME}"
echo "Build number is: ${env.BUILD_NUMBER}"
    
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
def mavenHome = tool name: 'maven 3.9.3'

stage('CheckOutCode'){
git branch: 'development', credentialsId: 'eb9ffc53-b2de-4192-bce4-448c57aafe65', url: 'https://github.com/mss-junebatch2023-pramirez1977/maven-web-application.git'    
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"    
}

stage('UploadArtifactsIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcatServer'){
sshagent(['504fba46-f09d-4f60-b119-1b3ea9e11dba']){
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.41.24:/opt/apache-tomcat-9.0.78/webapps/"
}

}

}
