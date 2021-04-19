pipeline {
  
   agent {
        label 'agent_java'
      }
  
  stages {
    
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
            sh "curl -u admin:admin --upload-file target/*.war 'http://10.10.20.31/repository/depot_test/app${BUILD_NUMBER}.war'"
          }
      }
            
  }
}
