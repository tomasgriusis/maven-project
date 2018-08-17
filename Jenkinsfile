pipeline {
	agent any

	parameters {
		string(name: 'tomcat', defaultValue: 'localhost:8081', description: 'Staging Server')
		string(name: 'tomcat-prod', defaultValue: 'localhost:8082', description: 'Production Server')
	}

	environment {
    	TOMCAT_CREDENTIALS = credentials('tomcat')
	}

	triggers {
		pollSCM('* * * * * ')
	}

	tools {
		maven 'maven-3.5.4'
	}	

	stages{
		stage('Build'){
			steps {
				sh 'mvn clean package'
			}
			
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
		stage('Deployments'){
			parallel{
				stage('Deploy to Staging'){
					steps {
						sh "cp -i **/target/*.war /opt/tomcat/webapps"
					}
				}
		
				stage('Deploy to Production'){
					steps {
						sh "cp -i **/target/*.war /opt/tomcat-prod/webapps"
					}
				}
			}	
		}
	}
}
