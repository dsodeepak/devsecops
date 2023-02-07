pipeline {
  agent any 
  tools {
    maven 'maven'
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
    
    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/dsodeepak/devsecops.git > trufflehog'
        sh 'cat trufflehog'
      }
    }    

    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }

   stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@172.31.59.244:/devsec/apache-tomcat-9.0.71/webapps/webapp.war'
              }      
           }       
    }
    

    }
}
