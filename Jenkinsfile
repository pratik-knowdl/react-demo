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
                #!/bin/bash
                export NVM_DIR="/home/ubuntu/.nvm"
                if [ -f "/home/ubuntu/.nvm/nvm.sh" ]; then
                    . /home/ubuntu/.nvm/nvm.sh
                fi
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
                #!/bin/bash
                export NVM_DIR="/home/ubuntu/.nvm"
                if [ -f "/home/ubuntu/.nvm/nvm.sh" ]; then
                    . /home/ubuntu/.nvm/nvm.sh
                fi
                npm install
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                #!/bin/bash
                export NVM_DIR="/home/ubuntu/.nvm"
                if [ -f "/home/ubuntu/.nvm/nvm.sh" ]; then
                    . /home/ubuntu/.nvm/nvm.sh
                fi
                npm run docs:build
                '''
            }
        }

        stage('Deploy to S3') {
            steps {
                withAWS(region: 'us-east-2', credentials: 'aws_cred_pratik') {
                    sh 'aws s3 sync docs/.vitepress/dist/ s3://3pi-dev/'
                }
            }
        }

        stage('Create CloudFront Invalidation') {
            steps {
                withAWS(region: 'us-east-2', credentials: 'aws_cred_pratik') {
                    sh '''
                    #!/bin/bash
                    invalidationId=$(aws cloudfront create-invalidation \
                        --distribution-id E3JY84G8VFB0K \
                        --paths "/*" \
                        --query 'Invalidation.Id' \
                        --output text)
                    echo "CloudFront Invalidation Created: ${invalidationId}"
                    '''
                }
            }
        }
    }
}
