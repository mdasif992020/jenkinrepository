node
{
  //  echo "Build Number is:  ${env.BUILD_NUMBER}"
  //  properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))])
  stage('CheckoutCode')  
  {
      git branch: 'development', credentialsId: '782e5c03-1c33-43ba-996e-8e93dc59a1a3', url: 'https://github.com/mdasif992020/maven-web-application.git'
  }
    
    stage('Build')
    {
      sh  "${mavenHome}/bin/mvn clean package"
    }
    
    stage('ExecuteSonarQubeReport')
    {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    
    stage('UploadArtifactsIntoNexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
    stage('Deploy WAR')
    {
     sshagent(['TomcatServerCredentials']) 
     {
     sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.235.113.118:/opt/apache-tomcat-9.0.38/webapps/"
     }}
     stage("Email Notification")
     {
         mail bcc: '', body: '''Build Completed!  
Success!

Thanks
Asif''', cc: '', from: '', replyTo: '', subject: 'Build Done!', to: 'mdasif_99@yahoo.com'
     }
    }
