pipeline {
    agent any 

    stages {
        stage('Build') {

            agent {
                docker {
                    image '18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Lets start building"
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm runn build
                    ls -la
            }
        }
    }
}