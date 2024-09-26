pipeline {
    agent any

    stages {
        stage('Notify GitHub (Pending)') {
            steps {
                githubNotify context: 'CI', status: 'PENDING', description: 'Build is starting'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    try {
                        sh 'npm test'
                        githubNotify context: 'CI', status: 'SUCCESS', description: 'Tests passed'
                    } catch (Exception e) {
                        githubNotify context: 'CI', status: 'FAILURE', description: 'Tests failed'
                        throw e // Re-throw the error to fail the pipeline
                    }
                }
            }
        }
    }

    post {
        failure {
            githubNotify context: 'CI', status: 'FAILURE', description: 'Build failed'
        }
        success {
            githubNotify context: 'CI', status: 'SUCCESS', description: 'Build succeeded'
        }
    }
}
