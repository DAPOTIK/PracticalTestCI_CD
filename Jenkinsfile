pipeline {
    agent any

    stages {

        stage('Environment') {
            steps {
                bat 'python --version'
                bat 'pip --version'
                bat 'git --version'
            }
        }

        stage('Install test dependencies') {
            steps {
                bat 'python -m venv .venv'
                bat '.venv\\Scripts\\python.exe -m pip install --upgrade pip'
                bat '.venv\\Scripts\\python.exe -m pip install -r backend\\requirements-test.txt'
                bat '.venv\\Scripts\\python.exe -m playwright install chromium'
            }
        }

        stage('Verify test tools') {
            steps {
                bat '.venv\\Scripts\\python.exe -m pytest --version'
                bat '.venv\\Scripts\\python.exe -m playwright --version'
            }
        }
    }
}
