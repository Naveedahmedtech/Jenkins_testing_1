pipeline {
    agent any

    environment {
        GIT_REPO = 'git@github.com:Naveedahmedtech/Jenkins_testing_1.git'
        GIT_CREDENTIALS = 'github-pat'
    }

    stages {
        stage('Notify GitHub (Pending)') {
            steps {
                script {
                    githubNotify context: 'CI', status: 'PENDING', description: 'Build is starting',
                                 repo: GIT_REPO, credentialsId: GIT_CREDENTIALS
                }
            }
        }

        stage('Checkout Code') {
            steps {
                checkout scm 
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
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
        failure {
            githubNotify context: 'CI', status: 'FAILURE', description: 'Build failed',
                         repo: GIT_REPO, credentialsId: GIT_CREDENTIALS
        }
        success {
            githubNotify context: 'CI', status: 'SUCCESS', description: 'Build succeeded',
                         repo: GIT_REPO, credentialsId: GIT_CREDENTIALS
        }
    }
}
