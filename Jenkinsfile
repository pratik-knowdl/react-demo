pipeline {
    agent any

    environment {
        NVM_DIR = "/home/ubuntu/.nvm"
        PATH = "${NVM_DIR}/versions/node/v20.16.0/bin:${env.PATH}"
    }

    stages {
        stage('Environment Check') {
            steps {
                sh '''
                export NVM_DIR="/home/ubuntu/.nvm"
                [ -s "$NVM_DIR/nvm.sh" ] && \\ . "$NVM_DIR/nvm.sh"
                node -v
                npm -v
                '''
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/pratik-knowdl/react-demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                export NVM_DIR="/home/ubuntu/.nvm"
                [ -s "$NVM_DIR/nvm.sh" ] && \\ . "$NVM_DIR/nvm.sh"
                npm install
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                export NVM_DIR="/home/ubuntu/.nvm"
                [ -s "$NVM_DIR/nvm.sh" ] && \\ . "$NVM_DIR/nvm.sh"
                npm run docs:build
                '''
            }
        }

        stage('Deploy to S3') {
            steps {
                withAWS(region: 'us-east-2', credentials: 'aws_cred_pratik') {
                    sh 'aws s3 sync docs/.vitepress/dist/ s3://3pi-dev/ --delete'
                }
            }
        }
    }
}
