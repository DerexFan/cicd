pipeline {
    agent any // Runs on any available executor

    environment {
        APP_NAME = "my-cool-app"
        CURRENT_VERSION = "1.0"
        SERVER_CREDENTIALS = credentials('bd98efb1-c6b8-4b66-969e-6220ebb1a85c')
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Pulling code from Git...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Compiling the application...'
                echo "current app: $APP_NAME, and current version: $CURRENT_VERSION"
                // Example for a Node project; use ./mvnw for Java, etc.
                //sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                //sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to Staging server...'
                withCredentials([
                    usernamePassword(credentials: 'SERVER_CREDENTIALS', usernameVariable: USER, passwordVariable: PWD)
                    ]) {
                        sh "some script username:${USER} and the password: ${PWD}"
                }
                // This is where you'd run a shell script or ssh command
                //sh './deploy.sh'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
        }
        success {
            echo 'Deployment successful! Party time.'
        }
        failure {
            echo 'Build failed. Check the logs immediately!'
        }
    }
}
