pipeline {
    agent any /*{
        docker{
            image 'node:18-alpine'
            reuseNode true  
        }
    }*/

    stages {
        // This is a comment 
        /*
        stage('Build') {
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
        } */
        stage('Test') {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true  
                    args '-u root:root'
                }
            }
            steps {
                sh '''
                test -f build/index.html
                echo 'test stage two'
                npm test
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.57.0-noble'
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