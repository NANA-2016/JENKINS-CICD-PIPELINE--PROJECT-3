
pipeline {
    agent any
    environment {
        IMAGE_NAME = 'jenkins-html-project'
        CONTAINER_NAME = 'jenkins-html-container'
        HOST_PORT = '8081'
    }
    stages {
        stage('Test SSH Access') {
            steps {
                sshagent(credentials: ['git']) {
                    sh 'ssh -o StrictHostKeyChecking=no -T git@github.com || true'
                }
            }
        }
        stage('Checkout Code') {
            steps {
                sshagent(credentials: ['git']) {
                    git url: 'git@github.com:NANA-2016/JENKINS-CICD-PIPELINE--PROJECT-3.git', branch: 'main'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }
       stage('Run Docker Container') {
    steps {
        script {
            sh "docker stop ${CONTAINER_NAME} || true"
            sh "docker rm ${CONTAINER_NAME} || true"
            sh "docker run -d -p ${HOST_PORT}:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
        }
    }
}
    }
    post {
        always {
            echo 'Cleaning up...'
            script {
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
                sh "docker rmi ${IMAGE_NAME} || true"
            }
        }
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
>>>>>>> 83d6718 (clear jenkinsfile)
