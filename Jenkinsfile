pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    environment {
        COMPOSE_FILE = '/home/tgnazmi/docker-container/docker-compose.yml'
        INFRA_DIR    = '/home/tgnazmi/docker-container'
    }

    stages {
        stage('Pull Latest') {
            steps {
                dir("${INFRA_DIR}") {
                    sh 'git pull origin main'
                }
            }
        }

        stage('Pull Frontend') {
            steps {
                dir("${FRONTEND_DIR}") {
                    sh 'git pull origin main'
                }
            }
        }
        
        stage('Pull Backend') {
            steps {
                dir("${BACKEND_DIR}") {
                    sh 'git pull origin main'
                }
            }
        }

        stage('Deploy Infrastructure') {
            steps {
                sh "docker compose -f ${COMPOSE_FILE} up -d --build"
            }
        }
    }

    post {
        failure {
            echo 'Infrastructure deployment failed!'
        }
        success {
            echo 'Infrastructure deployed successfully.'
        }
    }
}
