pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/deploy'
    }

    triggers {
        githubPush()
    }

    stages {

        stage('Build') {
            steps {
                echo 'Building project with Gradle...'
                sh '''
                    chmod +x gradlew
                    ./gradlew clean build
                '''
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
