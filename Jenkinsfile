#pipeline {
 #   agent any
#
 #   environment {
  #      IMAGE_NAME = 'jenkins-html-project'
   #     CONTAINER_NAME = 'jenkins-html-container'
    #    HOST_PORT = '8082'
    }#
#
 #   stages {
  #      stage('Build Docker Image') {
   #         steps {
    #            sh "docker build -t ${IMAGE_NAME} ."
     #       }
      #  }
#
 #       stage('Run Docker Container') {
  #          steps {
   #             sh "docker stop ${CONTAINER_NAME} || true"
    #            sh "docker rm ${CONTAINER_NAME} || true"
     #           sh "docker run -d -p ${HOST_PORT}:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
      #      }
       # }
    #}
#
pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                checkout scm
            }
        }

        stage('Serve HTML on Port 8082') {
            steps {
                sh '''
                    nohup python3 -m http.server 8082 > server.log 2>&1 &
                    sleep 5
                '''
                echo 'HTML server started on port 8082'
            }
        }
    }
}

    post {
        always {
            echo 'Cleaning up...'
            sh "docker stop ${CONTAINER_NAME} || true"
            sh "docker rm ${CONTAINER_NAME} || true"
            sh "docker rmi ${IMAGE_NAME} || true"
        }
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
