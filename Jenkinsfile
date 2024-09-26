pipeline {
    agent any

    environment {
        GIT_REPO = 'git@github.com:Naveedahmedtech/Jenkins_testing_1.git' // Your GitHub repo
        GIT_CREDENTIALS = 'github-pat' // Use the credentials ID from the previous step
    }

    stages {
        stage('Notify GitHub (Pending)') {
            steps {
                script {
                    // Set GitHub commit status to pending at the beginning of the build
                    githubNotify context: 'CI', status: 'PENDING', description: 'Build is starting',
                                 repo: GIT_REPO, credentialsId: GIT_CREDENTIALS
                }
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
                        // Notify GitHub about successful build
                        githubNotify context: 'CI', status: 'SUCCESS', description: 'Tests passed',
                                     repo: GIT_REPO, credentialsId: GIT_CREDENTIALS
                    } catch (Exception e) {
                        // Notify GitHub about failed build
                        githubNotify context: 'CI', status: 'FAILURE', description: 'Tests failed',
                                     repo: GIT_REPO, credentialsId: GIT_CREDENTIALS
                        throw e // Re-throw the error to fail the pipeline
                    }
                }
            }
        }
    }

    post {
        failure {
            // Notify GitHub about failure in post-actions
            githubNotify context: 'CI', status: 'FAILURE', description: 'Build failed',
                         repo: GIT_REPO, credentialsId: GIT_CREDENTIALS
        }
        success {
            // Notify GitHub about success in post-actions
            githubNotify context: 'CI', status: 'SUCCESS', description: 'Build succeeded',
                         repo: GIT_REPO, credentialsId: GIT_CREDENTIALS
        }
    }
}
