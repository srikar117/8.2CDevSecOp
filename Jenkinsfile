pipeline {
    agent any

    environment {
        PATH = "/opt/homebrew/bin:/Users/srikar/.docker/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }

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
                sh 'docker build -t nodejs-goof:${BUILD_NUMBER} .'
            }
        }

        stage('Code Quality') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh 'sonar-scanner -Dsonar.token=$SONAR_TOKEN'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker compose -p goof-staging up -d'
                sh 'sleep 15'
                sh 'curl -f http://localhost:3001 || (echo "Staging health check failed" && exit 1)'
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