pipeline {
    agent any

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
                    ls -la
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
                    mkdir -p test-results
                    test -f build/index.html
                    npm test
                    ls -lh test-results/
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.53.1-jammy'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install serve
                    mkdir -p playwright-results
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test --reporter=html
                '''
            }
        }
    }

    post {
        always {
            echo 'Listing files to confirm junit output location:'
            sh 'find . -type f'
            junit 'jest-results/junit.xml'
        }
    }
}
