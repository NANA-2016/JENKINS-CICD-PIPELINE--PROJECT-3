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
                script {
                    checkout([$class: 'GitSCM',
                        branches: [[name: "*/${BRANCH}"]],
                        userRemoteConfigs: [[url: "${REPO_URL}"]]
                    ])
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Run Docker Container') {
            steps {
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
                sh "docker run -d -p ${HOST_PORT}:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
            }
        }
    }

#    post {
 #       always {
  #          echo 'Cleaning up...'
   #         catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
    #            sh "docker stop ${CONTAINER_NAME} || true"
     #           sh "docker rm ${CONTAINER_NAME} || true"
      #          sh "docker rmi ${IMAGE_NAME} || true"
       #     }
        #}
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
