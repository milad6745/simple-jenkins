pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = "docker.io"
        DOCKER_IMAGE = "milad6745/flask-app"
        TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/milad6745/simple-jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE:$TAG .'
                }
            }
        }

        stage('Scan with Anchore') {
            steps {
                script {
                    sh '''
                    anchore-cli image add $DOCKER_IMAGE:$TAG
                    anchore-cli image wait $DOCKER_IMAGE:$TAG
                    anchore-cli evaluate check $DOCKER_IMAGE:$TAG
                    '''
                }
            }
        }

        stage('Deploy') {
            when {
                expression {
                    // شرط فقط اگر Anchore OK بود
                    def result = sh(script: "anchore-cli evaluate check $DOCKER_IMAGE:$TAG | grep pass || true", returnStdout: true).trim()
                    return result.contains("pass")
                }
            }
            steps {
                echo "Deploying Application..."
                sh './deploy.sh'  // اینجا می‌تونی دستور kubectl apply یا docker run بذاری
            }
        }
    }
}
