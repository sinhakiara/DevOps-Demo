pipeline {
    agent any

    environment {
        CONTAINER_NAME    = "webserver"
        IMAGE_NAME        = "httpd:latest"
        HOST_PORT         = "3000"
        CONTAINER_PORT    = "80"   // Apache listens on 80 by default
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/sinhakiara/DevOps-Demo'
            }
        }

        stage('Cleanup Old Container') {
            steps {
                script {
                    try {
                        echo "Stop and remove container: ${CONTAINER_NAME}"
                        sh "docker rm -f ${CONTAINER_NAME}"
                    } catch (Exception e) {
                        echo "Container was not running: ${e.getMessage()}"
                    }               
                }
            }
        }

        stage('Run New Container') {
            steps {
                echo "Starting new webserver container..."
                sh """
                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        -p ${HOST_PORT}:${CONTAINER_PORT} \
                        ${IMAGE_NAME}
                """
                echo "Container started successfully!"
            }
        }
    }
}
