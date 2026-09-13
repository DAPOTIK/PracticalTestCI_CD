pipeline {
    agent any

    stages {


        stage('Start app') {
            steps {
                bat 'docker-compose down || exit 0'
                bat 'docker-compose up -d --build'
            }
        }

        stage('Setup') {
            steps {
                bat 'python -m venv .venv'
                bat '.venv\\Scripts\\python.exe -m pip install -r backend\\requirements-test.txt'
                bat '.venv\\Scripts\\python.exe -m pip install -r backend\\requirements.txt'
                bat '.venv\\Scripts\\python.exe -m playwright install chromium'
            }
        }

        stage('Backend tests') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {

                   bat '.venv\\Scripts\\python.exe -m pytest backend\\tests'
                }
            }
        }

        stage('Frontend E2E tests') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    bat '.venv\\Scripts\\python.exe -m pytest frontend\\e2e\\tests --alluredir=allure-results --junitxml=frontend-test-results.xml'
                }
            }
        }
    }

    post {
        always {
            bat 'docker-compose down'

            junit testResults: 'backend-test-results.xml', allowEmptyResults: true
            junit testResults: 'frontend-test-results.xml', allowEmptyResults: true

            archiveArtifacts artifacts: 'allure-results/**/*', allowEmptyArchive: true
        }
    }
}
