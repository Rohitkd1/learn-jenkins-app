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
                    ls -la
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                    

                '''
            }
        }
    }


    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
