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
//   stage ('Check-Git-Secrets') {
//     steps {
//       sh 'rm trufflehog || true'
//       sh 'docker run gesellix/trufflehog --json https://github.com/dsodeepak/devsecops.git > trufflehog'
//       sh 'cat trufflehog'
//     }
//   }
  stage ('Source-Composition-Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://github.com/dsodeepak/devsecops/blob/master/owasp-dependency-check1.sh" '
         sh 'chmod +x owasp-dependency-check1.sh'
         sh 'bash owasp-dependency-check1.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
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
