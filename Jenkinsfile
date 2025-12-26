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
                        sh "sudo docker rm -f ${CONTAINER_NAME}"
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
                    sudo docker run -d \
                        --name ${CONTAINER_NAME} \
                        -p ${HOST_PORT}:${CONTAINER_PORT} \
                        ${IMAGE_NAME}
                """
                echo "Container started successfully!"
            }
        }
    }
    post {
        always {
            echo "Pipeline execution completed."
        }
        success {
            echo "Webserver is LIVE at: http://localhost:${HOST_PORT}"
            echo "Test it with: curl http://localhost:${HOST_PORT}"
        }
        failure {
            echo "Pipeline failed. Check the console output above."
            // Optional: show container logs on failure for easier debugging
            sh 'docker logs ${CONTAINER_NAME} || true'
        }
        cleanup {
            // Optional extra safety: remove container even if pipeline was aborted
            sh 'docker rm -f ${CONTAINER_NAME} || true'
        }
    }
}
