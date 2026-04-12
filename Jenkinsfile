//node starting
node {
    def mavenhomepath = tool name: "maven-3.9.0"
echo "git branch Name: ${env.BRANCH_NAME}"
echo "build number: ${env.BUILD_NUMBER}"
    try {

        stage('git checkout') {
            git branch: 'dev', url: 'https://github.com/Govardhan7882/maven-webapplication-project-kkfunda.git'
        }

        stage('maven compile') {
            sh "${mavenhomepath}/bin/mvn compile"
        }

        stage('maven build') {
            sh "${mavenhomepath}/bin/mvn clean package"
        }

        stage('sq report') {
            sh "${mavenhomepath}/bin/mvn sonar:sonar"
        }

        stage('nexus deploy') {
            sh "${mavenhomepath}/bin/mvn deploy"
        }

        stage('Deploy to Tomcat') {
            withCredentials([usernamePassword(
                credentialsId: 'tomcat-cred',
                usernameVariable: 'USERNAME',
                passwordVariable: 'PASSWORD'
            )]) {

                sh """
                curl -u "$USERNAME:$PASSWORD" \
                --upload-file target/maven-web-application.war \
                "http://13.234.120.65:8080/manager/text/deploy?path=/maven-web-application&update=true"
                """
            }
        }

        
    }
