pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/mnt/liferay/data/deploy' // mapped folder in Docker Compose
    }

    triggers {
        githubPush() // Trigger on GitHub push via webhook
    }

    stages {
        stage('Checkout') {
            steps {
                // Use SSH deploy key from Jenkins credentials
                sshagent(credentials: ['github-deploy-key']) {
                    git branch: 'main',
                        url: 'git@github.com:Khushwant09/liferay-demo.git'
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building project with Gradle...'
                sh './gradlew clean build'
            }
        }

        stage('Deploy to Liferay') {
            steps {
                echo "Copying artifacts to Liferay deploy folder..."
                sh 'mkdir -p ${DEPLOY_DIR}'
                sh 'cp modules/*/build/libs/*.jar ${DEPLOY_DIR}/ || true'
                sh 'cp modules/*/build/libs/*.war ${DEPLOY_DIR}/ || true'
            }
        }
    }

    post {
        success {
            echo 'Build & deploy completed successfully!'
        }
        failure {
            echo 'Build or deploy failed!'
        }
    }
}
