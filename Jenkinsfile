pipeline {
    agent any

    environment {
        IMAGE_NAME = 'angular-todo-app'
        IMAGE_TAG  = "build-${BUILD_NUMBER}"
        CONTAINER_NAME = 'angular-todo-container'
        HOST_PORT = '8090'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/veera-yerramsetty-usa/todo-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t $IMAGE_NAME:$IMAGE_TAG .
                    docker tag $IMAGE_NAME:$IMAGE_TAG $IMAGE_NAME:latest
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Add your test commands here (e.g. ng test --watch=false)'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    # Stop and remove existing container if running
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true

                    # Run new container
                    docker run -d \
                        --name $CONTAINER_NAME \
                        -p $HOST_PORT:80 \
                        --restart unless-stopped \
                        $IMAGE_NAME:latest
                '''
            }
        }

        stage('Cleanup Old Images') {
            steps {
                sh 'docker image prune -f'
            }
        }
    }

    post {
        success {
            echo "Deployment successful! App running at http://localhost:${HOST_PORT}"
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}
