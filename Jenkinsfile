pipeline {
    agent any

    environment {
        REPO_URL = ''  // آدرس گیتهاب خودت
        BRANCH   = 'main'                                  // برنچ مورد نظر
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
       
        }

        stage('Run Containers (Optional)') {
            steps {
                sh 'docker-compose up -d'
            }
        }
    }

    post {
        success {
            echo '✅ Build and deploy successful!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
