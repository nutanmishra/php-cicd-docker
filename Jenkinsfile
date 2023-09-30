pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                git 'https://github.com/Kennibravo/jenkins-laravel.git'
                sh 'composer install'
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
            }
        }

        stage('Test') {
            steps {
                echo 'Running Test phase.'
            }
        }

        stage('QA') {
            steps {
                echo 'Running QA phase.'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Running Deploy phase.'
            }
        }

        stage('Monitor') {
            steps {
                echo 'Running moniti phase.'
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed!'
        }
        success {
            echo 'Pipeline success!'
        }
    }
}
