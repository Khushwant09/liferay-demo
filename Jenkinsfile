pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/deploy'  // This maps to your Liferay containers
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code...'
                git branch: 'main',
                    url: 'git@github.com:Khushwant09/liferay-demo.git',
                    credentialsId: 'github-deploy-key'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project with Gradle 8.5...'
                sh 'gradle clean build'
            }
        }

        stage('Deploy to Liferay') {
            steps {
                echo 'Copying artifacts to Liferay deploy folder...'
                sh '''
                    mkdir -p ${DEPLOY_DIR}
                    cp modules/*/build/libs/*.jar ${DEPLOY_DIR}/ 2>/dev/null || true
                    cp modules/*/build/libs/*.war ${DEPLOY_DIR}/ 2>/dev/null || true
                '''
            }
        }
    }

    post {
        success {
            echo 'Build & deploy completed successfully'
        }
        failure {
            echo 'Build or deploy failed'
        }
    }
}
