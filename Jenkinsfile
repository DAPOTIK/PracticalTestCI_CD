pipeline {
    agent any

    stages {

        stage('Environment') {
            steps {
                bat 'py --version'
                bat 'py -m pip --version'
                bat 'git --version'
            }
        }
    }
}
