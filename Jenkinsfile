pipeline {
    agent any
    stages {
        stage ("Verify tooling") {
            steps {
                sh '''
                    docker info
                    docker version
                    docker compose version
                '''
            }
        }
        stage ("Clear all running docker containers") {
            steps {
                script {
                    try {
                        sh 'docker rm -f ${docker ps  -a -q}'
                    } catch (Exception e) {
                        echo 'No running container to clear up...'
                    }
                }
            }
        }
        stage ("Run docker compose") {
            steps {
                sh 'sudo docker compose up -d'
            }
        }
    }
}