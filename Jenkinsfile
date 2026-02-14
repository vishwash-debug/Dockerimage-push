pipeline {
    agent any

    environment {
        IMAGE_NAME = "myimg"
        IMAGE_TAG = "${BUILD_NUMBER}"
        CONTAINER_NAME = "webapp"
        DOCKER_CREDS = credentials('dockerhub-creds')
    }

    stages {

        stage('CODE') {
            steps {
                git url: "https://github.com/iammay786/dockerimgpush.git", branch: "main"
            }
        }

        stage('BUILD') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('IMAGE_CHECK') {
            steps {
                sh '''
                trivy image --severity CRITICAL --exit-code 0 $IMAGE_NAME:$IMAGE_TAG || true
                '''
            }
        }

        stage('IMAGE_PUSH') {
            steps {
                sh '''
                echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_CREDS_USR --password-stdin
                docker push $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }

        stage('DEPLOY') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                docker run -d --name $CONTAINER_NAME -p 80:5000 $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
}
