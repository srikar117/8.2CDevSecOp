pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/srikar117/8.2CDevSecOp.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '/opt/homebrew/bin/npm install'
            }
        }
        stage('Run Tests') {
            steps {
                sh '/opt/homebrew/bin/npm test || true'
            }
        }
        stage('Generate Coverage Report') {
            steps {
                sh '/opt/homebrew/bin/npm run coverage || true'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                sh '/opt/homebrew/bin/npm audit || true'
            }
        }
    }
}