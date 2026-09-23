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
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
        }

        stage('Build') {
            steps {
                sh '/Users/srikar/.docker/bin/docker build -t nodejs-goof:${BUILD_NUMBER} .'
            }
        }
    }

    post {
        always {
            emailext(
                subject: "Pipeline ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build status: ${currentBuild.currentResult}\n\nCheck console output at ${env.BUILD_URL}",
                to: 'maddukurisrikar@gmail.com',
                attachLog: true
            )
        }
    }
}