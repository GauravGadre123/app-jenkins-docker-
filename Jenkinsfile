pipeline {
    agent any 
    stages {
        stage('Compile and Clean') { 
            steps {
             sh "mvn clean compile"
            }
        }
       

        stage('deploy') {  
            steps {
                sh "mvn package"
            }
        }
 
      
        stage('Build Docker image'){
            steps {
              
                sh 'docker build -t  gauravgadre123/docker_jenkins_springboot:${BUILD_NUMBER} .'
            }
        }

        stage('Docker Login'){
            
            steps {
                
                 withCredentials([string(credentialsId: 'DockerId1', variable: 'Dockerpwd')]) 
                 {
                       sh "docker login -u gauravgadre123 -p ${Dockerpwd}"
                     
                 }
            }                
        }
        
         stage('Restart Docker'){
            steps {
                sh 'sudo service docker restart'
            }
        }
        

        stage('Docker Push'){
            steps {
                sh 'docker push gauravgadre123/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
        
          stage('Docker Image Pull'){
            steps {
                sh 'docker pull gauravgadre123/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
         
       
        stage('Docker deploy'){
            steps {
              
                sh 'docker run -d -e SERVER_PORT=9090  -p 9191:9090  -it gauravgadre123/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }

        
        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}

