pipeline {
    agent {
        docker {
            image 'composer:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'composer install'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'vendor/bin/phpunit'
            }
        }
    }
}
