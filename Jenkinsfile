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
            
        stage('Publication du binaire') {

          steps {
            sh "curl -u admin:admin --upload-file target/*.jar 'http://10.10.20.31:8081/repository/depot_test/rondoudou${BUILD_NUMBER}.jar'"        
          }

        }
  }
}
