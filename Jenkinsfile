pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    bat 'npm install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    bat 'npm test'
                }
            }
        }
    }

    post {
        success {
            script {
                echo 'Tests passed successfully.'
            }
        }

        failure {
            script {
                echo 'Tests failed.'
            }
        }
    }
}
