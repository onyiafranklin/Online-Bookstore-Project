pipeline {
    agent any

    environment {
        TOMCAT_PUBLIC_IP = '51.44.169.119'
    }
    
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ("Deploy to Tomcat"){
            steps {
                sshagent(['jenkins-key']) {
                    sh "scp -o StrictHostKeyChecking=no **/*.war ubuntu@${TOMCAT_PUBLIC_IP}:/opt/tomcat/webapps"
                    sh 'ssh -t -t ubuntu@${TOMCAT_PUBLIC_IP} -o strictHostKeyChecking=no "rm -rvf /opt/tomcat/webapps/ROOT.war && mv /opt/tomcat/webapps/*.war /opt/tomcat/webapps/ROOT.war"'
                }
            }
        }
    }
}
