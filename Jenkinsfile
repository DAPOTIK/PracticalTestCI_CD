pipeline {
    agent any

    stages {
        stage('Setup') {
            steps {
                bat 'python -m venv .venv'
                bat '.venv\\Scripts\\python.exe -m pip install -r backend\\requirements-test.txt'
                bat '.venv\\Scripts\\python.exe -m playwright install chromium'
            }
        }

        stage('Start app') {
            steps {
                bat '"C:\\Users\\parak\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker-compose.exe" up -d --build'
            }
        }

        stage('Tests') {
            steps {
                bat '.venv\\Scripts\\python.exe -m pytest frontend\\e2e\\tests --alluredir=allure-results --junitxml=test-results.xml'
            }
        }
    }

    post {
        always {
            bat '"C:\\Users\\parak\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker-compose.exe" down'
            junit testResults: 'test-results.xml', allowEmptyResults: true
            archiveArtifacts artifacts: 'allure-results/**/*', allowEmptyArchive: true
        }
    }
}
