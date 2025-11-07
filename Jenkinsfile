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

        stage('COPY_WAR_TO_S3') {
            steps {
                sh '''
                    aws s3 mb s3://jenkinspipelines3/ --region us-east-1 || true
                    aws s3 cp /mnt/project1/target/LoginWebApp.war s3://jenkinspipelines3/
                '''
            }
        }

        stage('DEPLOY_TO_SLAVE_1') {
            agent {
                node {
                    label 'slave-1'
                }
            }
            steps {
                sh '''
                    aws s3 cp s3://jenkinspipelines3/LoginWebApp.war /mnt/project1/target/
                    echo "WAR file deployed on slave-1 successfully."
                '''
            }
        }
    }
}
