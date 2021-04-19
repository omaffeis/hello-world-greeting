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
            
  }
}
