pipeline {
    agent { label "${LABEL_NAME}" }

    options {
        timestamps()
        buildDiscarder(logRotator(daysToKeepStr: '1', numToKeepStr: '2'))
    }

    environment {
        IMAGE_NAME = "myimg"
        IMAGE_TAG = "${BUILD_NUMBER}"
        CONTAINER_NAME = "webapp"
    }

    stages {

        stage('CODE') {
            steps {
                git url: "https://github.com/vishwash-debug/Dockerimage-push.git", branch: "main"
            }
        }

        stage('BUILD IMAGE') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('STOP OLD CONTAINER') {
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                '''
            }
        }

        stage('RUN CONTAINER') {
            steps {
                sh '''
                docker run -d -p 8085:5000 --name $CONTAINER_NAME $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }

    }

    post {
        success {
            echo "Deployment Succe
