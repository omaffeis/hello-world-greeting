pipeline {
  
  agent none
  
  stages {
    
    stage('Compilation et tests') {

      agent {
        label 'agent_java'
      }

      stages {
  
        stage('Test unitaire & publication') {
    
          steps {
            sh 'mvn test'
          }
      
          post {
      
            always {        
              junit 'target/surefire-reports/*.xml'
            }
        
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
    
    stage('Tests de déploiement') {
      
      agent {
        label 'agent_tomcat'
      }
      
      stages {
        
        stage('Téléchargement du binaire') {
          
          steps {
            sh "wget -P /home/jenkins/tomcat/webapps http://10.10.20.31:8081/repository/depot_test/rondoudou${BUILD_NUMBER}.jar"
            sh "mv /home/jenkins/tomcat/webapps/rondoudou${BUILD_NUMBER}.jar /home/jenkins/tomcat/webapps/rondoudou.jar"
          }
 
        }
               
        stage ('Validation de l\'application') {
  
          steps {
    
            sh "curl -u admin:admin --upload-file /home/jenkins/tomcat/webapps/rondoudou.jar 'http://10.10.20.31:8081/repository/hello_fiable/rondoudou_fiable${BUILD_NUMBER}.jar'"
            sh "curl -u admin:admin --upload-file /home/jenkins/tomcat/webapps/rondoudou.jar 'http://10.10.20.31:8081/repository/hello_livrable/dernier_rondoudou_fiable.jar'"
  
          }
  
        }
        
      }
      
    }
    
    stage('Creation de l\'image') {
      
      agent {
        label 'agent_docker'
      }
      
      stages {
        
        stage('Téléchargement du binaire') {
          
          steps {
            sh 'wget -P /home/jenkins/docker/tomcat_app http://10.10.20.31:8081/repository/hello_livrable/dernier_rondoudou_fiable.jar'
            sh 'mv /home/jenkins/docker/tomcat_app/dernier_rondoudou_fiable.jar /home/jenkins/docker/tomcat_app/rondoudou.jar'
          }
          
        }
          
        stage('Compilation de l\'image') {
      
          steps {
            sh 'docker build -t tomcat_app /home/jenkins/docker/tomcat_app'
          }
          
        }
        
        stage('Stockage de l\'image') {
              
          steps {
            sh "docker tag tomcat_app reeban/tomcat_app:${BUILD_NUMBER}"
            sh 'docker tag tomcat_app reeban/tomcat_app'
            sh 'docker login -u reeban -p Shaymin122'
            sh "docker push reeban/tomcat_app:${BUILD_NUMBER}"
            sh 'docker push reeban/tomcat_app'
          }
         
        }
      }
    }
  }
}
