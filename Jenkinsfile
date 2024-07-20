pipeline {
    agent {
        docker {
            image 'composer:latest'
            args '-u root:root' // Run as root to avoid permission issues, adjust as needed
        }
    }
    environment {
        COMPOSER_HOME = '/root/composer' // Example environment variable
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    try {
                        // Clean workspace before starting
                        deleteDir()
                        // Checkout the code from Git
                        checkout scm
                    } catch (Exception e) {
                        echo "Error during checkout: ${e.getMessage()}"
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
        stage('Prepare') {
            steps {
                script {
                    try {
                        // Install dependencies
                        sh '#!/bin/sh -xe\ncomposer install'
                    } catch (Exception e) {
                        echo "Error during preparation: ${e.getMessage()}"
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    try {
                        // Perform the build
                        sh '#!/bin/sh -xe\ncomposer install'
                    } catch (Exception e) {
                        echo "Error during build: ${e.getMessage()}"
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    try {
                        // Run tests
                        sh '#!/bin/sh -xe\nmkdir -p logs\n./vendor/bin/phpunit --log-junit logs/unitreport.xml tests'
                        // Verify if the report is generated
                        sh 'ls -l logs/'
                    } catch (Exception e) {
                        echo "Error during testing: ${e.getMessage()}"
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
        }
    }
    post {
        always {
            script {
                try {
                    // Publish test results
                    node {
                        junit 'logs/unitreport.xml'
                    }
                } catch (Exception e) {
                    echo "Error during post stage: ${e.getMessage()}"
                }
            }
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
