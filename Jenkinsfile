pipeline {
    agent any

    environment {
        BACKEND_ENV = './backend/venv'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Sytrax2721/GrpAssign-SoftwareDeployment.git'
            }
        }
stage('Install Backend Dependencies') {
            steps {
                script {
                    bat 'python -m venv %BACKEND_ENV%'
                    bat '%BACKEND_ENV%\\Scripts\\pip install -r backend/requirements.txt'
                }
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                bat 'cd frontend && yarn install'
            }
        }

        stage('Run Backend Tests') {
            steps {
                script {
                    bat '%BACKEND_ENV%\\Scripts\\pytest backend_test.py'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                bat 'cd frontend && yarn build'
            }
        }

        stage('Deploy to Staging') {
            steps {
                bat 'docker-compose -f docker-compose.staging.yml up -d'
            }
        }

        stage('Deploy to Production') {
            steps {
                input 'Approve deployment to production?'
                bat 'docker-compose -f docker-compose.production.yml up -d'
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}