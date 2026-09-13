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
                bat 'docker-compose up -d --build'
            }
        }

        stage('Tests') {
            steps {
                bat '.venv\\Scripts\\python.exe -m pytest froФntend\\e2e\\tests --alluredir=allure-results --junitxml=test-results.xml'
            }
        }
    }

    post {
        always {
            bat 'docker-compose down'
            junit testResults: 'test-results.xml', allowEmptyResults: true
            archiveArtifacts artifacts: 'allure-results/**/*', allowEmptyArchive: true
        }
    }
}
