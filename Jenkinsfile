pipeline {
    agent any

    environment {
        IMAGE_NAME     = "python-app"
        IMAGE_TAG      = "v1"
        DOCKERHUB_USER = "mohammed12mujahed"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker version
                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Tag Image') {
            steps {
                sh '''
                docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                    docker logout
                    '''
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
