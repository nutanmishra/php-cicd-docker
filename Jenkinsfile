pipeline {
    agent any

    environment {
        PHP_IMAGE = 'php:7.4-fpm'
        MYSQL_IMAGE = 'mysql:5.7'
        APP_NAME = 'php-cicd-docker'
        DB_NAME = 'test'
        DB_USER = 'root'
        DB_PASS = '2rD#Ua9Z^Mdy'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Build and run the MySQL container
                    docker.image(MYSQL_IMAGE).withRun('-e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=${DB_NAME} -e MYSQL_USER=${DB_USER} -e MYSQL_PASSWORD=${DB_PASS} --name ${DB_NAME}') { c ->
                        // Build the PHP container with your application code
                        docker.image(PHP_IMAGE).inside("--link ${DB_NAME}:db --name ${APP_NAME}") {
                            sh 'composer install' // Install PHP dependencies
                            sh 'phpunit' // Run PHPUnit tests
                        }
                    }
                }
            }
        }

        stage('Deploy') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                // Here you can add deployment steps such as pushing to a registry
                // or deploying your application to a production server.
                // You can also clean up the containers, networks, and volumes if needed.
                script {
                    def appContainer = docker.image(PHP_IMAGE)
                    def dbContainer = docker.image(MYSQL_IMAGE)

                    appContainer.stop()
                    appContainer.remove()
                    dbContainer.stop()
                    dbContainer.remove()
                }
            }
        }
    }

    post {
        failure {
            // Notify or perform actions on pipeline failure
            echo 'Pipeline failed!'
        }
    }
}
