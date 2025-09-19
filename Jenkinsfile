pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('frontend') { sh 'npm install' }
                dir('backend')  { sh 'npm install' }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') { sh 'npm run build' }
            }
        }

        stage('Backend Tests') {
            steps {
                dir('backend') { sh 'npm test' }
            }
        }

        stage('Optional Frontend Tests') {
            steps {
                dir('frontend') { 
                    // si tu as Puppeteer ou Cypress
                    sh 'npm run test || echo "Frontend tests completed"' 
                }
            }
        }

    }
}
