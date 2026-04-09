//node starting
node {
    def mavenhomepath = tool name: "maven-3.9.0"

    try {
        // 🟠 Build Started (Amber)
        slackSend channel: '#testing-slack', color: '#FFC107',
        message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' - ${env.BUILD_URL}"

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
                "http://52.66.93.166:8080/manager/text/deploy?path=/maven-web-application&update=true"
                """
            }
        }

        // 🟢 Success (Green)
        slackSend channel: '#testing-slack', color: '#36a64f',
        message: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' - ${env.BUILD_URL}"

    } catch (Exception e) {

        // 🔴 Failed (Red)
        slackSend channel: '#testing-slack', color: '#FF0000',
        message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' - ${env.BUILD_URL}"

        throw e

    } finally {
        // 📌 Always executes
        slackSend channel: '#testing-slack',
        message: "COMPLETED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' with status: ${currentBuild.currentResult}"
    }
}
