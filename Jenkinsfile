pipeline {
    agent {
        docker {
            image 'composer:latest'
            args '-v /your/host/path:/your/container/path' // If you need to mount volumes
        }
    }
    environment {
        COMPOSER_HOME = '/root/composer' // Example environment variable
    }
    stages {
        stage('Prepare') {
            steps {
                // Clean workspace before starting
                deleteDir()
                // Install dependencies
                sh '#!/bin/sh -xe\ncomposer install'
            }
        }
        stage('Build') {
            steps {
                // Perform the build
                sh '#!/bin/sh -xe\ncomposer install'
            }
        }
        stage('Test') {
            steps {
                // Run tests
                sh '#!/bin/sh -xe\n./vendor/bin/phpunit --log-junit logs/unitreport.xml tests'
            }
        }
    }
    post {
        always {
            // Publish test results
            junit 'logs/unitreport.xml'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
