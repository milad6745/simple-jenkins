pipeline {
    agent any

    stages {
        stage('Build Docker Compose') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Run Docker Compose') {
            steps {
                sh 'docker-compose up -d'
            }
        }
    }
}
