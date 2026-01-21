pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/deploy'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                sshagent(credentials: ['github-deploy-key']) {
                    git branch: 'main',
                        url: 'git@github.com:Khushwant09/liferay-demo.git'
                }
            }
        }

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

                    # Copy OSGi modules
                    cp modules/*/build/libs/*.jar ${DEPLOY_DIR}/ 2>/dev/null || true

                    # Copy WARs (themes, hooks, etc.)
                    cp modules/*/build/libs/*.war ${DEPLOY_DIR}/ 2>/dev/null || true
                '''
            }
        }
    }

    post {
        success {
            echo 'Build & deploy completed successfully '
        }
        failure {
            echo 'Build or deploy failed '
        }
    }
}
