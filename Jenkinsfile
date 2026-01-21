pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/deploy'  // Maps to your Liferay container deploy folder
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                git branch: 'main',
                    url: 'git@github.com:Khushwant09/liferay-demo.git',
                    credentialsId: 'github-deploy-key'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project with Gradle 8.5...'
                sh '''
                    chmod +x ./gradlew
                    ./gradlew clean buildBundle --no-daemon
                '''
            }
        }

        stage('Deploy to Liferay') {
            steps {
                echo 'Copying OSGi modules to Liferay deploy folder...'
                sh '''
                    mkdir -p ${DEPLOY_DIR}
                    find modules -type f -name "*-bundle.jar" -exec cp {} ${DEPLOY_DIR}/ \\;
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
