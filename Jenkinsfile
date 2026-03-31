@"
pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    environment {
        APP_NAME = 'my-java-app'
        BUILD_VERSION = "1.0.\${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                echo '========== STAGE 1: Checkout =========='
                checkout scm
                echo 'Code checkout completed successfully!'
            }
        }

        stage('Build') {
            steps {
                echo '========== STAGE 2: Build =========='
                echo "Building \${APP_NAME} version \${BUILD_VERSION}..."
                sh 'mvn clean package -DskipTests'
                echo 'Build completed successfully!'
            }
            post {
                failure {
                    echo 'BUILD FAILED! Stopping pipeline...'
                }
            }
        }

        stage('Test') {
            steps {
                echo '========== STAGE 3: Test =========='
                echo 'Running unit tests...'
                sh 'mvn test'
                echo 'All tests passed!'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
                failure {
                    echo 'TESTS FAILED! Check test reports.'
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
                echo '========== STAGE 4: Deploy =========='
                echo "Deploying \${APP_NAME} version \${BUILD_VERSION}..."
                echo 'Simulating deployment to staging server...'
                sh 'echo Deployment successful to STAGING environment'
                sh 'ls target/*.jar'
                echo 'Deployment simulation completed!'
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
            echo '========== Archiving Artifacts =========='
            archiveArtifacts artifacts: 'target/*.jar',
                             fingerprint: true,
                             allowEmptyArchive: true
        }
        success {
            echo '========================================='
            echo 'PIPELINE SUCCEEDED!'
            echo 'All stages completed successfully.'
            echo '========================================='
        }
        failure {
            echo '========================================='
            echo 'PIPELINE FAILED!'
            echo 'Check console output for details.'
            echo '========================================='
        }
        unstable {
            echo 'Pipeline is UNSTABLE - Some tests may have failed.'
        }
        aborted {
            echo 'Pipeline was ABORTED manually.'
        }
    }
}
"@ | Set-Content Jenkinsfile