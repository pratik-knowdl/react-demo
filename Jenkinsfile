pipeline {
    agent any

    tools {nodejs "node"}

    stages {
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
                rm -rf /var/www/html/*
                cp -r docs/.vitepress/dist/* /var/www/html/
                sudo systemctl reload nginx
                '''
            }
        }
    }
}

