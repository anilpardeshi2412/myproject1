pipeline {
			 agent {
        			 any {
            				customWorkspace '/mnt/project1'
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
		
		stage ('edit '){
		
				steps {
						sh '''
						mkdir test
						cd test
						cp -r /mnt/project/target/LoginWebApp.war .
						unzip LoginWebApp.war
						rm -rf LoginWebApp.war
						cd LoginWebApp
						perl -pi -e 's|DriverManager\.getConnection.*|DriverManager.getConnection("jdbc:mysql://database-1.c5mmc6ium69n.eu-north-1.rds.amazonaws.com:3306/mydb", "admin", "admin12345");|g' useerRegistration.jsp
						zip -r LoginWebApp.war *
						cp -r LoginWebApp.war /mnt/servers/apache-tomcat-10.1.49/webapps/

						'''
						}				
				}
	    }	
	

}
