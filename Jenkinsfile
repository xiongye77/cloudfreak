pipeline {
  agent any
    tools {
      maven 'maven3'
                 jdk 'jdk8'
    }
    stages {      
        stage('Build maven ') {
            steps { 
                    sh 'pwd'      
                    sh '/opt/apache-maven-3.6.3/bin/mvn  clean install package'
            }
        }
        
        stage('Copy Artifact') {
           steps { 
                   sh 'pwd'
		   sh 'cp -r target/*.jar docker'
           }
        }
         
        stage('Build docker image') {
           steps {
               script {         
                 def customImage = docker.build('dbaxy770928/java-maven', "./docker")
		       docker.withRegistry('https://registry.hub.docker.com', 'dockerhub'){
		       customImage.push("${env.BUILD_NUMBER}")}
                              
                }
            }
	 }
	    
	   stage('Helm deploy on Kubernetes ') {
            steps {           
                        sh 'pwd'
                        sh 'cp -R helm/* .'
		        sh 'ls -ltr'
                        sh 'pwd'
                        sh '/usr/local/bin/helm upgrade --install petclinic-app petclinic  --set image.repository=registry.hub.docker.com/dbaxy770928/java-maven --set image.tag=${BUILD_NUMBER} '
              			
            }           
        }
       }
}
