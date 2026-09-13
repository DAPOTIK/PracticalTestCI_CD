pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Environment') {
            steps {
                bat 'python --version'
                bat 'pip --version'
                bat 'git --version'
            }
        }
    }
}