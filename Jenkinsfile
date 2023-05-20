node{
    echo "job name is :${env.JOB_NAME}"
    echo "Build name is :${env.BUILD_NUMBER}"
    echo "Node name is :${env.NODE_NAME}"
    echo "Jenkins home name is :${env.JENKINS_HOME}"
    def mavenHome = tool name:"maven3.8.7"
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
    stage('checkoutcode'){
        git branch: 'development', credentialsId: '41286bfb-45f7-4aef-9dff-0aa537135d0b', url: 'https://github.com/pradeep-498/maven-web-application.git'
    }
    
    stage('Build'){
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('ExecuteSonarQubeReport'){
       sh "${mavenHome}/bin/mvn clean sonar:sonar package"
    }
    stage('DeployAppintoTomcat'){
        sshagent(['92bc3aef-7020-44c6-855f-335ea339b7dd']) {
          sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.15.63:/opt/apache-tomcat-9.0.74/webapps/"
}

}
   /*
    stage('UploadArtifactReportintoNexus'){
       sh "${mavenHome}/bin/mvn clean deploy"
    }
    */
}
