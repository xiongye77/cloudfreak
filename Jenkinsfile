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
	    
	   stage('Build on k8 ') {
            steps {           
                        sh 'pwd'
                        
		        sh 'ls -ltr'
                        sh 'pwd'
                        sh '/usr/local/bin/helm upgrade --install petclinic-app petclinic  --set image.repository=registry.hub.docker.com/prawinkorvi/petclinic --set image.tag="${env.BUILD_NUMBER}" '
              			
            }           
        }
       }
}
