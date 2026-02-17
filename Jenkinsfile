pipeline {
    agent any

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

        stage('RUN CONTAINER') {
            steps {
                sh 'docker run -d --name $CONTAINER_NAME $IMAGE_NAME:$IMAGE_TAG'
            }
        }
        post {
            success {
                archiveArtifacts artifacts: '*.tar', followSymlinks: false
                
            }
            failure {
                echo "deployment is failed, Please check logs"
            }
            
        }
    }
}
