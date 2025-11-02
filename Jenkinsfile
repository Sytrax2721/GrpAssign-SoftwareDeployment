pipeline {
    agent any

    environment {
        // Define backend virtual environment for Python
        BACKEND_ENV = './backend/venv'
    }

    stages {
        // Stage 1: Checkout the code from GitHub repository
        stage('Checkout') {
            steps {
                // Checkout the latest code from GitHub
               git 'https://github.com/Sytrax2721/GrpAssign-SoftwareDeployment.git'
            }
        }

        // Stage 2: Install backend dependencies
        stage('Install Backend Dependencies') {
            steps {
                script {
                    // Create a virtual environment for the backend (Python)
                    sh 'python3 -m venv $BACKEND_ENV'
                    // Install Python dependencies listed in requirements.txt
                    sh '$BACKEND_ENV/bin/pip install -r backend/requirements.txt'
                }
            }
        }

        // Stage 3: Install frontend dependencies
        stage('Install Frontend Dependencies') {
            steps {
                // Install JavaScript dependencies (for frontend, React or similar)
                sh 'cd frontend && yarn install'
            }
        }

        // Stage 4: Run backend tests
        stage('Run Backend Tests') {
            steps {
                script {
                    // Run backend tests using pytest (or any testing tool you are using)
                    sh '$BACKEND_ENV/bin/pytest backend_test.py'
                }
            }
        }

        // Stage 5: Build the frontend
        stage('Build Frontend') {
            steps {
                // Build the frontend (assuming React or similar JS framework)
                sh 'cd frontend && yarn build'
            }
        }

        // Stage 6: Deploy to Staging (Docker container for testing)
        stage('Deploy to Staging') {
            steps {
                // Deploy the application to a staging environment (using Docker)
                sh 'docker-compose -f docker-compose.staging.yml up -d'
            }
        }

        // Stage 7: Manual approval for Production Deployment
        stage('Deploy to Production') {
            steps {
                input 'Approve deployment to production?'  // Wait for manual approval
                // Deploy to production (using Docker Compose for AWS EC2 or any server)
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
