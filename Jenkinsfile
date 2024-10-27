pipeline {
  agent any
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
            echo "PATH = ${PATH}"
            echo "M2_HOME = ${M2_HOME}"
            '''
      }
    }
    
    stage ('Check Git') {
      steps {
        sh 'rm truffel || true'
        sh 'docker run trufflesecurity/trufflehog --json git https://github.com/D-Lesev/webapp.git > truffel'
        sh 'cat truffel'
      }
    }
    
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    
    stage ('Deploy-To-Tomcat') {
      steps {
          sshagent (['tomcat']) {
          sh 'scp -o StrictHostKeyChecking=no target/*.war tomcat@172.24.162.107:/home/tomcat/prod/apache-tomcat-10.1.31/webapps/webapp.war'
        }
        }
      }
    }
  }
