void setBuildStatus(String message, String state) {
    step([
        $class: 'GitHubCommitStatusSetter',
        reposSource: [
            $class: 'ManuallyEnteredRepositorySource',
            url: 'https://github.com/Naveedahmedtech/Jenkins_testing_1'
        ],
        contextSource: [
            $class: 'ManuallyEnteredCommitContextSource',
            context: 'ci/jenkins/build-status'
        ],
        errorHandlers: [
            [$class: 'ChangingBuildStatusErrorHandler', result: 'UNSTABLE']
        ],
        statusResultSource: [
            $class: 'ConditionalStatusResultSource',
            results: [
                [$class: 'AnyBuildResult', message: message, state: state]
            ]
        ]
    ])
}

pipeline {
    agent any

    tools {
        nodejs 'nodeJS'
    }

    stage('PR Event') {
            steps {
                script {
                    if (env.CHANGE_ID) {
                        echo "This is a Pull Request: ${env.CHANGE_ID}"
                    } else {
                        echo 'This is not a Pull Request'
                    }
                }
            }
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
            setBuildStatus('Build succeeded', 'SUCCESS')
        }
        failure {
            setBuildStatus('Build failed', 'FAILURE')
        }
    }
}
