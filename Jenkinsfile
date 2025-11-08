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
						aws s3 mb s3://ani142514251
						aws s3 cp /mnt/project1/target/LoginWebApp.war s3://ani142514251/
							'''
						}				
				}
	    }	
	
post { 
        success { 
           build "s3_s1_deploy"

        }
    }	
}
