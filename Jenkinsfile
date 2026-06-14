pipeline {
    agent any

    environment {
        EC2_USER     = "ec2-user"
        EC2_HOST     = credentials('ec2-host')
        DOCKER_USER  = credentials('dockerhub-creds')
        IMAGE        = "${DOCKER_USER_USR}/devops-project-01"
    }

    stages {

        stage('1 - Build JAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('2 - Build & Push Docker Image') {
            steps {
                sh "docker build -t ${IMAGE}:${BUILD_NUMBER} ."
                sh "echo ${DOCKER_USER_PSW} | docker login -u ${DOCKER_USER_USR} --password-stdin"
                sh "docker push ${IMAGE}:${BUILD_NUMBER}"
                sh "docker logout"
            }
        }

        stage('3 - Deploy to EC2') {
            steps {
                withCredentials([sshUserPrivateKey(
                    credentialsId: 'ec2-ssh-key',
                    keyFileVariable: 'SSH_KEY'
                )]) {
                    sh """
                        ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no \
                        ${EC2_USER}@${EC2_HOST} '
                            docker pull ${IMAGE}:${BUILD_NUMBER}
                            docker stop app || true
                            docker rm app || true
                            docker run -d --name app -p 8080:8080 \
                                --restart unless-stopped \
                                ${IMAGE}:${BUILD_NUMBER}
                        '
                    """
                }
            }
        }

        stage('4 - Health Check') {
            steps {
                sh "sleep 10"
                sh "curl http://${EC2_HOST}:8080 || true"
            }
        }
    }

    post {
        success { echo "App is live at http://${EC2_HOST}:8080" }
        failure { echo "Pipeline failed. Check the logs above." }
        always  { sh "docker rmi ${IMAGE}:${BUILD_NUMBER} || true" }
    }
}
