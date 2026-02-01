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

        stage('Rebuild Traefik') {
            steps {
                sh "docker compose -f ${COMPOSE_FILE} up -d --no-deps traefik"
            }
        }

        stage('Rebuild Jenkins') {
            steps {
                sh "docker compose -f ${COMPOSE_FILE} build jenkins"
            }
        }

        stage('Recreate Jenkins') {
            steps {
                sh "docker compose -f ${COMPOSE_FILE} up -d --no-deps jenkins"
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
