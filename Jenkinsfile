pipeline {
  
  agent none
  
  stages {
   
    stage (‘Compilation et tests’) {
      agent {
            label 'agent_java'
      }

      stage('Test unitaire') {
          steps {
            sh 'mvn test'
          }
      }

      stage('Compilation') {
          steps {  
            sh 'mvn -B -DskipTests clean package'
          }
      }
    
      stage ('Publication du binaire') {
          steps {
            sh "curl -u admin:admin --upload-file target/*.war 'http://10.10.20.31:8081/repository/depot_test/app${BUILD_NUMBER}.war'"
          }
      }
    }
    
    stage (‘Tests de déploiement’) {

      agent {
        label ‘agent_tomcat’
      }

      stages {
        stage (‘Livraison continue’) {
          
          stage('Téléchargement du binaire') {
            steps {
              sh "wget -P /home/jenkins/tomcat/webapps http://10.10.20.31:8081/repository/depot_test/app${BUILD_NUMBER}.war"
              sh "mv /home/jenkins/tomcat/webapps/app${BUILD_NUMBER}.war /home/jenkins/tomcat/webapps/app.war"
            }
          }
        }
        
        stage('test de performance') {
          steps {
            sh '/home/jenkins/apache-jmeter/bin/jmeter.sh -n -t ./jmeter.jmx -l /home/jenkins/test_report.jtl'
          }
        }
      }
    }
  }
}
