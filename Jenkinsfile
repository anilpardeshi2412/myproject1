pipeline {
    agent {
        node {
            label 'built-in'
            customWorkspace '/mnt/project1'
        }
    }

    stages {

        stage('CLEAN_OLD_M2') {
            steps {
                sh 'rm -rf /root/.m2/repository'
            }
        }

        stage('MAVEN_BUILD') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('EDIT_WAR') {
            steps {
                sh """
                    mkdir -p test
                    cd test
                    cp -r /mnt/project1/target/LoginWebApp.war .
                    unzip -o LoginWebApp.war
                    rm -f LoginWebApp.war
                    cd LoginWebApp

                    # ✅ Safe edit (escape properly for Groovy)
                    perl -pi -e 's|DriverManager\\.getConnection.*|DriverManager.getConnection("jdbc:mysql://database-1.c5mmc6ium69n.eu-north-1.rds.amazonaws.com:3306/mydb", "admin", "velocity");|g' userRegistration.jsp

                    zip -r LoginWebApp.war *
                    cp -r LoginWebApp.war /mnt/servers/apache-tomcat-10.1.49/webapps/
                """
            }
        }
    }
}
