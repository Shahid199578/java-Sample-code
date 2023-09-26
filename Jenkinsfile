pipeline {
    agent any
    tools {
        maven 'Maven-Tool'
    }
    environment {
        
        your_tomcat_ip = '18.204.9.68'
        WAR_FILE = 'Shahidt-app'
        
    }
    stages {
        stage("Git Checkout") {
            steps {
                git branch: 'master', credentialsId: 'git', url: 'https://github.com/Shahid199578/java-Sample-code.git'
            }
        }
        stage("Maven Build") {
            steps {
                sh "mvn clean package"
            }
        }
        stage("Archive Artifact") {
            steps {
                archiveArtifacts 'target/*.war'
            }
        }
        
        stage ('Deploying Artifact') {
            steps {
                script {
                    sshagent(credentials: ['tomcat-server']) {
                        
                        sh "scp -o StrictHostKeyChecking=no $WORKSPACE/*/*.war ubuntu@${env.your_tomcat_ip}:/tmp/"
                        sh "ssh ubuntu@${env.your_tomcat_ip} 'sudo mv /tmp/*.war /opt/tomcat/webapps/${env.WAR_FILE}.war'"
                        sh "ssh ubuntu@${env.your_tomcat_ip} 'sudo systemctl restart tomcat.service'"
                        
                    }
                }
            }
        }
    }
}
