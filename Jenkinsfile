 1  pipeline {
 2      agent any
 3
 4      environment {
 5          IMAGE_NAME = 'jenkins-html-project'
 6          CONTAINER_NAME = 'jenkins-html-container'
 7          HOST_PORT = '8081'
 8      }
 9
10      stages {
11          stage('Test SSH Access') {
12              steps {
13                  sshagent(credentials: ['git']) {
14                      sh 'ssh -o StrictHostKeyChecking=no -T git@github.com || true'
15                  }
16              }
17          }
18
19          stage('Checkout Code') {
20              steps {
21                  sshagent(credentials: ['git']) {
22                      git url: 'git@github.com:NANA-2016/JENKINS-CICD-PIPELINE--PROJECT-3.git', branch: 'main'
23                  }
24              }
25          }
26
27          stage('Build Docker Image') {
28              steps {
29                  script {
30                      sh "docker build -t ${IMAGE_NAME} ."
31                  }
32              }
33          }
34
35          stage('Run Docker Container') {
36              steps {
37                  script {
38                      sh "docker stop ${CONTAINER_NAME} || true"
39                      sh "docker rm ${CONTAINER_NAME} || true"
40                      sh "docker run -d -p ${HOST_PORT}:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
41                  }
42              }
43          }
44      }
45
46      post {
47          always {
48              echo 'Cleaning up...'
49              script {
50                  sh "docker stop ${CONTAINER_NAME} || true"
51                  sh "docker rm ${CONTAINER_NAME} || true"
52                  sh "docker rmi ${IMAGE_NAME} || true"
53              }
54          }
55          success {
56              echo '✅ Pipeline completed successfully!'
57          }
58          failure {
59              echo '❌ Pipeline failed.'
60          }
61      }
62  }
