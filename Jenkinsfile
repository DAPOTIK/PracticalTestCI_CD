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
    }
}
