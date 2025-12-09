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
                         sh 'mvn clean package '
            }
        }

        stage('DEPLOY DOCKER COMPOSE') {
            steps {
                echo '🚀 Deploying Docker containers...'
                sh '''
                    cd /mnt/project
                    docker-compose down || true
                    docker-compose up -d --build
                '''
            }
        }

        stage('WAIT FOR MYSQL INIT') {
            steps {
                echo '⏳ Waiting for MySQL initialization...'
                script {
                    def maxRetries = 15
                    def retryCount = 0
                    def mysqlReady = false

                    while (retryCount < maxRetries) {
                        def status = sh(
                            script: "docker exec mysql_container mysqladmin ping -h localhost -uadmin -pmysecurepassword --silent",
                            returnStatus: true
                        )

                        if (status == 0) {
                            echo "✅ MySQL is ready!"
                            mysqlReady = true
                            break
                        } else {
                            echo "🕒 MySQL not ready yet... waiting 5s (Attempt ${retryCount + 1}/${maxRetries})"
                            sleep(time: 5, unit: 'SECONDS')
                            retryCount++
                        }
                    }

                    if (!mysqlReady) {
                        error("❌ MySQL did not initialize in time.")
                    }
                }
            }
        }

        stage('VERIFY DEPLOYMENT') {
            steps {
                echo '🔍 Checking running containers...'
                sh 'docker ps'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment completed successfully!'
        }
        failure {
            echo '❌ Deployment failed. Check logs above.'
        }
    }
}
