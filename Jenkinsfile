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
				sh "rm -rf /root/.m2/repository"
				
			}
			
		}
	
		stage ('MAVEN_BUILD') {
		
			steps {
						
						sh "mvn clean package"
			
			}
			
		
		}
		
		stage ('COPY_WAR_TO_S3'){
		
				steps {
						
						aws s3 cp /mnt/project-myapp/target/LoginWebApp.war s3://jenkinspipelines3slave1/

						}
				
				}
	
	
	
	}
	post { 
	       agent {
				label {
						label "slave-1"					
		
		              }
		         }
        success { 
		
            sh '''
			aws s3 cp s3://jenkinspipelines3slave1/LoginWebApp.war /mnt/servers/apache-tomcat-10.1.48/webapps/
			
			'''
        }
    }
		
}
