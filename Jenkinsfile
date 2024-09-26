void setBuildStatus(String message, String state) {
    step([
        $class: 'GitHubCommitStatusSetter',
        reposSource: [
            $class: 'ManuallyEnteredRepositorySource',
            url: 'https://github.com/Naveedahmedtech/Jenkins_testing_1.git'
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

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    def scmVars = checkout([$class: 'GitSCM', branches: [[name: '*/master']], 
                        userRemoteConfigs: [[url: 'git@github.com:Naveedahmedtech/Jenkins_testing_1.git']]])
                    env.GIT_COMMIT = scmVars.GIT_COMMIT
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
            setBuildStatus('Build succeeded', 'SUCCESS')
        }
        failure {
            setBuildStatus('Build failed', 'FAILURE')
        }
    }
}
