pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {

        stage('Start app') {
            steps {
                bat 'docker-compose down || exit 0'
                bat 'docker-compose up -d --build'
            }
        }

        stage('Setup') {
            steps {
                bat 'if exist .venv rmdir /s /q .venv'
                bat 'python -m venv .venv'
                bat '.venv\\Scripts\\python.exe -m pip install -r backend\\requirements-test.txt'
                bat '.venv\\Scripts\\python.exe -m pip install -r backend\\requirements.txt'
                bat '.venv\\Scripts\\python.exe -m playwright install chromium'
                bat 'if not exist traces mkdir traces'
            }
        }

        stage('Backend tests') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    bat '.venv\\Scripts\\python.exe -m pytest backend\\tests --alluredir=allure-results --junitxml=backend-test-results.xml'
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
            allure includeProperties: false, results: [[path: 'allure-results']]

            archiveArtifacts artifacts: 'allure-results/**/*', allowEmptyArchive: true
        }
    }
}
