pipeline {
    agent any

    environment {
        DEPLOY_NODE_1 = "/deploy-node-1"
        DEPLOY_NODE_2 = "/deploy-node-2"
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
                echo 'Building Liferay module...'
                sh '''
                    chmod +x ./gradlew
                    ./gradlew clean buildBundle --no-daemon
                '''
            }
        }

        stage('Deploy to Liferay Nodes') {
            steps {
                echo 'Deploying JAR to BOTH nodes...'
                sh '''
                    ls -lh modules/**/build/libs/*.jar

                    cp -v modules/**/build/libs/*.jar "$DEPLOY_NODE_1"/
                    cp -v modules/**/build/libs/*.jar "$DEPLOY_NODE_2"/
                '''
            }
        }
    }

    post {
        success {
            echo 'Build & deployed to both nodes successfully'
        }
        failure {
            echo 'Build or deploy failed'
        }
    }
}
