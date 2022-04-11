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

        stage('Docker Push'){
            steps {
                sh 'docker push gauravgadre123/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
        
        stage('Docker deploy'){
            steps {
               
                sh 'docker run -itd -p 9082:9090 gauravgadre123/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }

        
        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}

