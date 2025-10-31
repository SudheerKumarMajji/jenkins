pipeline {
    agent any

    stages {
        /*
        stage('Build') {
            agent {
                docker {
                    image 'node:24-alpine'
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
        stage('Run tests'){
            parallel{
                stage('Test') {
            
             agent {
                docker {
                    image 'node:24-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                   cd build
                   ls -la
                   npm test
                '''
            }
        }

        stage('E2E') {
            
             agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.50.0-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                   npm install serve
                   node_modules/.bin/serve -s build
                   npx playwright test
                '''
            }
        }
            }


        }
        /*
        stage('Test') {
            
             agent {
                docker {
                    image 'node:24-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                   cd build
                   ls -la
                   npm test
                '''
            }
        }

        stage('E2E') {
            
             agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.50.0-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                   npm install serve
                   node_modules/.bin/serve -s build
                   npx playwright test
                '''
            }
        }
        */
    }

    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}
