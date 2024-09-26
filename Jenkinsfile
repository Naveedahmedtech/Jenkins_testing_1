pipeline {
    agent any

    tools {
        nodejs 'nodeJS'
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: '*/master']], 
                        userRemoteConfigs: [[url: 'git@github.com:Naveedahmedtech/Jenkins_testing_1.git']]])
                }
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
            githubNotify context: 'ci/jenkins/build-status', 
                         status: 'SUCCESS', 
                         description: 'Build and tests succeeded', 
                         repo: 'Naveedahmedtech/Jenkins_testing_1', 
                         sha: "${env.GIT_COMMIT}", 
                         credentialsId: 'github-pat'
        }
        failure {
            githubNotify context: 'ci/jenkins/build-status', 
                         status: 'FAILURE', 
                         description: 'Build or tests failed', 
                         repo: 'Naveedahmedtech/Jenkins_testing_1', 
                         sha: "${env.GIT_COMMIT}", 
                         credentialsId: 'github-pat'
        }
    }
}
