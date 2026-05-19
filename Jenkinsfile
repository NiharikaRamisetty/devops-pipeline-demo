pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "flask-cicd-app"
        CONTAINER_NAME = "flask-cicd-container"
        PORT = "5000"
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'
                bat "docker build -t ${DOCKER_IMAGE} ."
            }
        }
        
        stage('Stop Old Container') {
            steps {
                echo 'Stopping old container if it exists...'
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    bat "docker stop ${CONTAINER_NAME}"
                    bat "docker rm ${CONTAINER_NAME}"
                }
            }
        }
        
        stage('Run New Container') {
            steps {
                echo 'Deploying new container...'
                bat "docker run -d -p ${PORT}:5000 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}"
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully! The application is deployed.'
        }
        failure {
            echo 'Pipeline failed! Please check the logs.'
        }
    }
}
