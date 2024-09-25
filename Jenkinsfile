pipeline {
    agent any

    tools {
        nodejs 'nodeJS'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'git@github.com:Naveedahmedtech/Jenkins_testing_1.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test'  
            }
        }

        stage('Build Application') {
            steps {
                bat 'npm run build'
            }
        }
    }

    post {
        success {
            githubNotify context: 'Jenkins/NodeJS-Testing-Pipeline', status: 'SUCCESS', description: 'Build and tests succeeded'
        }
        failure {
            githubNotify context: 'Jenkins/NodeJS-Testing-Pipeline', status: 'FAILURE', description: 'Build or tests failed'
        }
    }
}
