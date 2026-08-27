pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '6263fff5-04eb-436b-bdd3-a5e46194ef28'
    }



    stages {
        stage('Build') {
            agent{
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

        stage('Test') {
            agent{
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


        stage('Deploy') {
            agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying to Production. Site id $NETLIFY_SITE_ID"
                '''
            }
        }



    }
    post{
        always{
            junit 'test-results/junit.xml'
        }







        
    }
}
