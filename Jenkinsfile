pipeline {
    agent any

    environment {
        BACKEND_DIR = "/home/ubuntu/backend"
        FRONTEND_DIR = "/var/www/frontend"
    }

    stages {

        stage('Clone') {
            steps {
                checkout scm
            }
        }

        stage('Install Backend') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Install Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy Backend') {
            steps {
                sh '''
                rm -rf $BACKEND_DIR
                mkdir -p $BACKEND_DIR
                cp -r backend/* $BACKEND_DIR/
                cd $BACKEND_DIR
                npm install
                pm2 restart backend || pm2 start server.js --name backend
                '''
            }
        }

        stage('Deploy Frontend') {
            steps {
                sh '''
                sudo rm -rf $FRONTEND_DIR/*
                sudo mkdir -p $FRONTEND_DIR
                sudo cp -r frontend/dist/* $FRONTEND_DIR/
                sudo systemctl restart nginx
                '''
            }
        }
    }
}
