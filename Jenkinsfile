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
                    sh 'python3 -m venv $BACKEND_ENV'
                    sh '$BACKEND_ENV/bin/pip install -r backend/requirements.txt'
                }
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                sh 'cd frontend && yarn install'
            }
        }

        stage('Run Backend Tests') {
            steps {
                script {
                    sh '$BACKEND_ENV/bin/pytest backend_test.py'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'cd frontend && yarn build'
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh 'docker-compose -f docker-compose.staging.yml up -d'
            }
        }

        stage('Deploy to Production') {
            steps {
                input 'Approve deployment to production?'
                sh 'docker-compose -f docker-compose.production.yml up -d'
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
