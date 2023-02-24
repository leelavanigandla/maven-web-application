node{

echo "Job name is: ${env.JOB_NAME}"
echo "Build Number is: ${env.BUILD_NUMBER}"
echo "Node Name is: ${env.NODE_NAME}"
echo "Jenkis Home dir is: ${env.JENKINS_HOME}"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '5', numToKeepStr: '')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

def mavenHome = tool name: "maven3.8.7"

stage('CheckoutCode'){
git branch: 'development', credentialsId: 'da89c62c-ac91-42d3-bab0-1e4df4a9d595', url: 'https://github.com/leelavanigandla/maven-web-application.git'
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

stage('DeployAppIntoTomcatServer'){
sshagent(['f946174c-0574-4afb-812d-b2b8846f4594']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.37.245:/opt/apache-tomcat-9.0.71/webapps/"
}
}



}



