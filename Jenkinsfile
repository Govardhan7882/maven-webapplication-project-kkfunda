pipeline
{
    agent any
    tools
    {
     maven "maven-3.9.0"
    }
    stages
    {
     stage('git checkout')
     {
       steps
       {
        git branch: 'dev', url: 'https://github.com/Govardhan7882/maven-webapplication-project-kkfunda.git'
       }
     }
    
    stage('maven build')
    {
     steps
     {
       sh "mvn clean package"
     }
    }
    stage('sq report')
    {
     steps
     {
       sh "mvn sonar:sonar"
     }
    }
    stage('deploy to nexus')
    {
      steps
      {
       sh "mvn deploy"
      }
    }
    stage('tomcat deploy')
    {
      steps
      {
       sh """

      curl -u govardhan:password \
--upload-file /var/lib/jenkins/workspace/go-declarativeway-pl/target/maven-web-application.war \
"http://13.234.17.115:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """
      }
    }

  } 


    post {
        success {
            script {
                sendSlackNotifications(currentBuild.result)
            }
        }

        failure {
            script {
                sendSlackNotifications(currentBuild.result)
            }
        }
    }
}
