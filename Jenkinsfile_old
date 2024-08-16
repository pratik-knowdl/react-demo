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

        stage('Deploy') {
            steps {
                sh '''
                sudo rm -rf /var/www/html/*
                sudo cp -r docs/.vitepress/dist/* /var/www/html/
                sudo systemctl reload nginx
                '''
            }
        }
    }
}

