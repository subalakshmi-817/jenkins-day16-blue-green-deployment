pipeline {
    agent any

    environment {
        BLUE_IMAGE = "day16-blue"
        GREEN_IMAGE = "day16-green"

        BLUE_CONTAINER = "day16-blue-container"
        GREEN_CONTAINER = "day16-green-container"

        BLUE_PORT = "8081"
        GREEN_PORT = "8082"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Source code checked out from GitHub'
            }
        }

        stage('Build Blue Image') {
            steps {
                sh 'docker build -t ${BLUE_IMAGE} ./blue'
            }
        }

        stage('Build Green Image') {
            steps {
                sh 'docker build -t ${GREEN_IMAGE} ./green'
            }
        }

        stage('Remove Old Containers') {
            steps {
                sh 'docker rm -f ${BLUE_CONTAINER} || true'
                sh 'docker rm -f ${GREEN_CONTAINER} || true'
            }
        }

        stage('Deploy Blue') {
            steps {
                sh 'docker run -d --name ${BLUE_CONTAINER} -p ${BLUE_PORT}:80 ${BLUE_IMAGE}'
            }
        }

        stage('Health Check Blue') {
            steps {
                sh 'sleep 5'
                sh 'curl -f http://localhost:${BLUE_PORT}'
            }
        }

        stage('Deploy Green') {
            steps {
                sh 'docker run -d --name ${GREEN_CONTAINER} -p ${GREEN_PORT}:80 ${GREEN_IMAGE}'
            }
        }

        stage('Health Check Green') {
            steps {
                sh 'sleep 5'
                sh 'curl -f http://localhost:${GREEN_PORT}'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'docker ps'
                echo 'Blue-Green deployment completed successfully!'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Blue-Green deployment completed.'
        }

        failure {
            echo 'FAILURE: Blue-Green deployment failed.'
        }
    }
}
