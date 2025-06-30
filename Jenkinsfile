pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-u 130:138'
                }
            }
            environment {
                NPM_CONFIG_CACHE = '.npm' // avoid permission issues with /.npm
            }
            steps {
                sh '''
                    ls -la 
                    node --version
                    npm --version
                    npm ci --loglevel=verbose
                '''
            }
        }
    }
}
