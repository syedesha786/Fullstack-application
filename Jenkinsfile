pipeline {
    agent any

    environment {
        // Make sure Docker commands can run
        DOCKER_HOST = 'unix:///var/run/docker.sock'
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo "Cloning repo..."
                git branch: 'main', url: 'https://github.com/syedesha786/Fullstack-application.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                echo "Building Docker images..."
                sh '''
                    docker-compose build --no-cache
                '''
            }
        }

        stage('Run Containers') {
            steps {
                echo "Stopping old containers and starting new ones..."
                sh '''
                    docker-compose down || echo "No containers to stop"
                    docker-compose up -d
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline finished successfully! Containers are up."
        }
        failure {
            echo "Pipeline failed. Check logs and fix errors."
        }
    }
}