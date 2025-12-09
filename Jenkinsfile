pipeline {
    agent any

    stages {
        stage('BUILD WAR') {
            steps {
                sh 'mvn clean package -f /mnt/project/pom.xml'
            }
        }

        stage('DEPLOY DOCKER COMPOSE') {
            steps {
                sh '''
                    cd /mnt/project
                    docker-compose down || true
                    docker-compose up -d --build
                '''
            }
        }
    }

    post {
        always {
            sh 'docker ps'
        }
    }
}
