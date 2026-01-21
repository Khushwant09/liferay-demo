pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/mnt/liferay/data/deploy'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                sshagent(['e37faf3c-48de-4292-a211-1090299331a0']) {
                    sh 'git clone -b main git@github.com:Khushwant09/liferay-demo.git workspace'
                    dir('workspace') {
                        sh 'git fetch --all'
                        sh 'git checkout main'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                dir('workspace') {
                    sh './gradlew clean build'
                }
            }
        }

        stage('Deploy to Liferay') {
            steps {
                echo "Copying artifacts to Liferay deploy folder..."
                sh 'mkdir -p ${DEPLOY_DIR}'
                sh 'cp workspace/modules/*/build/libs/*.jar ${DEPLOY_DIR}/ || true'
                sh 'cp workspace/modules/*/build/libs/*.war ${DEPLOY_DIR}/ || true'
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
