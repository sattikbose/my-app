pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    environment {
        APP_NAME = 'my-java-app'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'STAGE 1: Checkout'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'STAGE 2: Build'
                bat 'mvn clean package -DskipTests'
            }
            post {
                failure {
                    echo 'BUILD FAILED! Stopping pipeline...'
                }
            }
        }

        stage('Test') {
            steps {
                echo 'STAGE 3: Test'
                bat 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
                failure {
                    echo 'TESTS FAILED!'
                }
            }
        }

        stage('Deploy') {
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                echo 'STAGE 4: Deploy'
                echo 'Simulating deployment to staging server...'
                bat 'echo Deployment successful!'
            }
            post {
                failure {
                    echo 'DEPLOYMENT FAILED!'
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/*.jar',
                             fingerprint: true,
                             allowEmptyArchive: true
        }
        success {
            echo 'PIPELINE SUCCEEDED!'
        }
        failure {
            echo 'PIPELINE FAILED!'
        }
    }
}