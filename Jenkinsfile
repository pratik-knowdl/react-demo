pipeline {
    agent any

    environment {
        PATH = "/usr/local/lib/node/nodejs/bin:${env.PATH}"
    }

    stages {
	stage('Environment Check') {
            steps {
                sh 'echo $PATH'
                sh 'node -v'
                sh 'npm -v'
            }
        }
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/pratik-knowdl/react-demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run docs:build'
            }
        }

        stage('Deploy to S3') {
            steps {
                withAWS(region: 'us-east-2', credentials: 'aws_cred_pratik') {
                    sh 'aws s3 sync docs/.vitepress/dist/ s3://3pi-dev --delete'
                }
            }
        }
    }
}

