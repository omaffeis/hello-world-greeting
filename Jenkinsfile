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
          
          
        }
      }
    }
  }
}
