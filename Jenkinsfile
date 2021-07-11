node
{
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
    def mvnhome = tool name: "maven 3.6.2"
    stage('checkout code')
    {
        git credentialsId: '649cb1c7-953f-481d-919d-8dbf810a4cb1', url: 'https://github.com/mithun-sshan/maven-web-application.git'
    }
    /*
    stage('bulid')
    {
        sh "${mvnhome}/bin/mvn clean package"
    }
    */
    stage('sonarqube report')
    {
        sh "${mvnhome}/bin/mvn sonar:sonar"
    }
    stage('nexus repo')
    {
        sh "${mvnhome}/bin/mvn clean deploy"
    }
    stage('Deploy into Tomcat')
    {
        sshagent(['fbf1fdae-f20f-4781-b83c-7a9b98e68456'])
        {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@52.14.51.159:/opt/apache-tomcat-9.0.48/webapps/"
        }
    }
    stage('email notification')
    {
        mail bcc: '', body: 'build', cc: '', from: '', replyTo: '', subject: 'build info', to: 'shankarsh4494@gmail.com'
    }
}
