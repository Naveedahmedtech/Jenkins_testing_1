pipeline {
    agent any

    tools {
        nodejs 'nodeJS'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'git@github.com:Naveedahmedtech/NodeJS-Simple-Job.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test'  // Jenkins will fail if this step fails
            }
        }

        stage('Build Application') {
            steps {
                bat 'npm run build'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/build/**/*', allowEmptyArchive: true
        }
        failure {
            mail to: 'team@example.com', subject: "Build Failed", body: "Please check the Jenkins build logs."
        }
    }
}
