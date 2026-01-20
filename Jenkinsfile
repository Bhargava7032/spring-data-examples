pipeline {
    agent any

    tools {
        maven 'maven3'
    }

    options {
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out branch: ${env.BRANCH_NAME}"
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building branch: ${env.BRANCH_NAME}"
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests on branch: ${env.BRANCH_NAME}"
                sh 'mvn test'
            }
        }

        stage('Branch Info') {
            steps {
                echo "Branch Name  : ${env.BRANCH_NAME}"
                echo "Commit ID    : ${env.GIT_COMMIT}"
                echo "Build Number : ${env.BUILD_NUMBER}"
            }
        }

        stage('Deploy (Conditional)') {
            when {
                anyOf {
                    branch 'boot-next'
                    branch '1.x'
                }
            }
            steps {
                echo "Deploying branch: ${env.BRANCH_NAME}"
            }
        }
    }

    post {
        success {
            echo "Build SUCCESS for ${env.BRANCH_NAME}"
        }
        failure {
            echo "Build FAILED for ${env.BRANCH_NAME}"
        }
    }
}

