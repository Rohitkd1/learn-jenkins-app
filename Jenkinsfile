pipeline {
    agent any

    environment {
        HOME = "${WORKSPACE}"  // ✅ sets a safe, writable home for npm
    }

    stages {

        /*
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

        */

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    #test -f build/index.html
                    npm test
                    

                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.53.0-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    
                    npm install -g serve
                    serve -s build
                    npx playwright test
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
