pipeline {
    agent any

    environment {
        HOME = "${WORKSPACE}"  // ✅ sets a safe, writable home for npm
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Starting build"
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }
    }
}
