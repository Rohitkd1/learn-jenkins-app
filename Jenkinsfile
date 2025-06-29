pipeline {
    agent any

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
                    # --- FIX: Configure npm to use a cache directory within the workspace ---
                    # This tells npm to store its cache in a directory relative to the current workspace
                    # (e.g., /var/lib/jenkins/workspace/learn-jenkins-app/.npm-cache)
                    # which is writable by the Jenkins user, avoiding EACCES errors on /.npm.
                    npm config set cache ./.npm-cache --global

                    # --- OPTIONAL: Clean the npm cache ---
                    # This ensures a fresh cache for the new location and can help clear any corrupted
                    # entries from previous failed runs. It's safe to run after setting the new cache path.
                    npm cache clean --force

                    ls -la
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
                    # Ensure you have a test command configured in your package.json, e.g., "test": "jest --config jest.config.js --testResultsProcessor='jest-junit'"
                    npm test
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            // This step will only find test reports if the 'Test' stage completes successfully.
            // The previous 'No test report files were found' error was a symptom of the 'Build' failure.
            junit 'jest-results/junit.xml'
        }
    }
}
