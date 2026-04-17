pipeline {
    agent any // Runs on any available executor

    tools {
        maven 'Maven'
    }

    parameters {
        string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod')
        choice(name: 'VERSION', choices: ['1.0', '1.1', '1.2'], description: '')
        booleanParam(name: 'executeTests', defaultValue: true, description: '')
    }

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

        stage('init') {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Compiling the application...'
                echo "current app: $APP_NAME, and current version: $CURRENT_VERSION"
                // Example for a Node project; use ./mvnw for Java, etc.
                //sh 'npm install'
                script {
                    gv.buildApp()
                }
            }
        }

        stage('Test') {
            when {
                expression {
                    params.executeTests
                }
            }
            steps {
                echo 'Running unit tests...'
                //sh 'npm test'
                script {
                    gv.testApp()
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to Staging server...'
               withCredentials([
            usernamePassword(
                credentialsId: 'bd98efb1-c6b8-4b66-969e-6220ebb1a85c', 
                usernameVariable: 'USER', 
                passwordVariable: 'PWD'
            )
        ]) {
            // Note: Use single quotes for 'sh' to prevent Groovy from leaking secrets into logs
            // sh "some script username:${USER} and the password: ${PWD}"
        }
                // This is where you'd run a shell script or ssh command
                //sh './deploy.sh'
                echo "deploy the version is: ${VERSION}"

                input {
                    message "Select the environment to deploy to"
                    ok "Done"
                    parameters {
                        choice(name："ENV", choices: ['dev', 'staging', 'prod'], description: '')
                    }
                }

                script {
                    gv.deployApp()
                    echo "Deploying to ${ENV}"
                }
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
