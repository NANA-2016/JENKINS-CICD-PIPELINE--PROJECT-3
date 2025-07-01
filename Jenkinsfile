pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jenkins-html-project'
        CONTAINER_NAME = 'jenkins-html-container'
        HOST_PORT = '8082'
        REPO_URL = 'https://github.com/NANA-2016/JENKINS-CICD-PIPELINE--PROJECT-3.git'
        BRANCH = 'main'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Simpler declarative checkout
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "${BRANCH}"]],
                    userRemoteConfigs: [[url: "${REPO_URL}"]]
                ])
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Run Docker Container') {
            steps {
                // Stop and remove container if it exists
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
                // Run new container
                sh "docker run -d -p ${HOST_PORT}:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
            }
        }

        stage('Verify Container is Running') {
            steps {
                // Wait a few seconds for container to start
                sh "sleep 5"
                // Check container health via curl
                sh "curl -f http://localhost:${HOST_PORT} || (echo 'Health check failed' && exit 1)"
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
        // Uncomment this block later when you want automatic cleanup
        /*
        always {
            echo 'Cleaning up...'
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
                sh "docker rmi ${IMAGE_NAME} || true"
            }
        }
        */
    }
}
