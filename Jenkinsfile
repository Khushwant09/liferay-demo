pipeline {
    agent any

    environment {
        DEPLOY_DIR = '/deploy'  // Mount this to your Liferay container if needed
        GRADLE_OPTS = '-Dorg.gradle.daemon=false' // optional for CI builds
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
                    # Ensure gradle wrapper is executable
                    if [ -f ./gradlew ]; then
                        chmod +x ./gradlew
                        ./gradlew clean build --no-daemon
                    else
                        # Fallback to container-installed Gradle
                        gradle clean build
                    fi
                '''
            }
        }

        stage('Deploy to Liferay') {
            steps {
                echo 'Copying artifacts to Liferay deploy folder...'
                sh '''
                    mkdir -p ${DEPLOY_DIR}
                    
                    # Copy all OSGi modules
                    find modules -type f -name "*.jar" -exec cp {} ${DEPLOY_DIR}/ \\; || true

                    # Copy WARs for themes/hooks
                    find modules -type f -name "*.war" -exec cp {} ${DEPLOY_DIR}/ \\; || true
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
