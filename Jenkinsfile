pipeline {
    agent any

    environment {
        DEPLOY_DIR = "/deploy"
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

        stage('Deploy to Liferay') {
            steps {
                echo 'Deploying JAR to Liferay deploy folder...'
                sh '''
                    echo "JARs found:"
                    ls -lh modules/**/build/libs/*.jar

                    cp -v modules/**/build/libs/*.jar /deploy/
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
