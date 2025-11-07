pipeline {
			agent {
					label {
								label "built-in"
								customWorkspace "/mnt/project1"
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
						sh '''
						
						
						aws s3 cp /mnt/project1/target/LoginWebApp.war s3://jenkinspipelines3/
							'''
						}				
				}
	    stage ('slave-1'){
						 agent {
							label {
									label "slave-1"
									
							    }						 
						 }		
				steps {
						sh '''
						aws s3 cp /mnt/project1/target/LoginWebApp.war s3://jenkinspipelines3/
                             '''
						}				
				}	
	}		
}
