pipeline {
    agent any
    tools {
        maven 'Maven'
    }

    stages {
        stage('Intialize') {
            steps {
            sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
               ''' 
            }
        }
    stage ('Check-Git-Secrets') {
      steps {
        sh 'sudo docker run gesellix/trufflehog --json https://github.com/devopspushparaj/webapp.git >> trufflehog'
        sh 'cat trufflehog'
      }
    }
        stage('Build') {
            steps {
            sh 'mvn clean package'
            }
        }
        stage ('Deploy-To-Tomcat') {
            steps {
              sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/WebApp.war ubuntu@13.233.87.209:/home/ubuntu/prod/apache-tomcat-8.5.69/webapps/webapp.war'
               }      
          }       
      }
    }
}
