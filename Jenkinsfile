pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/deploy' // mapped folder shared with Liferay containers
    }

    triggers {
        // Trigger on Git push (webhook)
        pollSCM('H/2 * * * *') // optional fallback if webhook isn't configured
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
                // Example using Maven
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Liferay') {
            steps {
                echo "Copying artifacts to deploy folder..."
                sh 'cp target/*.jar ${DEPLOY_DIR}/'
                sh 'cp target/*.war ${DEPLOY_DIR}/'
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
