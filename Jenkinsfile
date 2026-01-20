pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/mnt/liferay/data/deploy' // mapped folder in Docker Compose
    }

    triggers {
        // Trigger on Git push (webhook)
        githubPush() // simplified if GitHub webhook is configured
        // pollSCM('H/2 * * * *') // optional fallback
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your_username/your_repo.git'
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
