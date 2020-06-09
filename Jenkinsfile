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
       }
}
