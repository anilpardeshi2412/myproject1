pipeline {
    agent {
        node {
            label 'built-in'
            customWorkspace '/mnt/project'
        }
    }

    stages {
        stage('BUILD WAR') {
            steps {
                echo 'Building WAR file...'
                sh 'mvn clean package -f /mnt/project/pom.xml'
            }
        }

        stage('DEPLOY DOCKER COMPOSE') {
            steps {
                echo 'Deploying with Docker Compose...'
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
            echo 'Listing running containers...'
            sh 'docker ps'
        }
    }
}
