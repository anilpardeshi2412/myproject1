
pipeline {
agent {
label {
		label "built-in"
		customWorkspace "/mnt/project-myapp"
		
		}
		}
		
	stages {
		
		stage ('CLEAN_OLD_M2') {
			
			steps {
				sh "rm -rf /home/saccount/.m2/repository"
				
			}
			
		}
	
		stage ('MAVEN_BUILD') {
		
			steps {
						
						sh "mvn clean package"
			
			}
			
		
		}
		
		stage ('COPY_WAR_TO_Server'){
		
				steps {
						
						sh "scp -i /halo.pem /mnt/project-myapp/target/LoginWebApp.war ec2-user@172.31.30.182:/mnt/servers/apache-tomcat-10.1.48/webapps/"

						}
				
				}
	
	
	
	}
		
}
