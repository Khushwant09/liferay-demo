pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/mnt/liferay/data/deploy' // mapped folder in Docker Compose
    }

    triggers {
        // Trigger on Git push (webhook)
        githubPush() // will trigger on GitHub webhook
        // pollSCM('H/2 * * * *') // optional fallback if webhook not configured
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'git@github.com:Khushwant09/liferay-demo.git',
                    credentialsId: 'e37faf3c-48de-4292-a211-1090299331a0'
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
