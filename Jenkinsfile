node
{
    def mavenHome = tool name: 'maven3.9.1'
    echo "The job name is: ${env.JOB_NAME}"
    echo "The build.no is: ${env.BUILD_NUMBER}"
    properties([buildDiscarder(logRotator(artifactNumToKeepStr: '5', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
     stage('Checkout Code')
        {
            git branch: 'development', credentialsId: '624e16dd-5f9b-4069-9658-ab6c227a77bd', url: 'https://github.com/YESSBank/maven-web-application.git'
        } //closing checkoutcode
    stage('Build')
    {
       sh "${mavenHome}/bin/mvn clean package"
    }//closing build
    stage('Execute SonarQube Report')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }//closing Execute SonarQube Report
    stage('Upload Artifacts into Nexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }//closing Execute SonarQube Report
    stage('Deploy Application')
    {
        sshagent(['04350f3f-486a-4f9a-bd3d-0bae01318603']) 
        {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.37.202:/opt/apache-tomcat-9.0.73/webapps/"
        }
    }//closing Deploy Application
}
