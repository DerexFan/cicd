pipeline {
    agent any // Runs on any available executor

    environment {
        APP_NAME = "my-cool-app"
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
