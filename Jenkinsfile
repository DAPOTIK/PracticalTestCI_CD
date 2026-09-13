```groovy
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

        stage('Backend tests') {
            steps {
                catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
                    bat '.venv\\Scripts\\python.exe -m pytest backend\\tests --junitxml=backend-test-results.xml'
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
            bat '"C:\\Users\\parak\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker-compose.exe" down'

            junit testResults: 'backend-test-results.xml', allowEmptyResults: true
            junit testResults: 'frontend-test-results.xml', allowEmptyResults: true

            archiveArtifacts artifacts: 'allure-results/**/*', allowEmptyArchive: true
        }
    }
}
```
