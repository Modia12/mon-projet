pipeline {
    agent any

    environment {
        WORK_DIR = "${env.WORKSPACE ?: '.'}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Frontend') {
            steps {
                script {
                    def frontendDir = "${WORK_DIR}/frontend"
                    echo "Build Frontend in ${frontendDir}"

                    dir(frontendDir) {
                        sh '''
                            echo "=== Frontend Build ==="
                            ls -la
                            # npm install && npm run build
                            echo "Simulate frontend build..."
                        '''
                    }
                }
            }
        }

        stage('Build Backend') {
            steps {
                script {
                    def backendDir = "${WORK_DIR}/backend"
                    echo "Build Backend in ${backendDir}"

                    dir(backendDir) {
                        sh '''
                            echo "=== Backend Build ==="
                            ls -la
                            # mvn clean install OR your backend build command
                            echo "Simulate backend build..."
                        '''
                    }
                }
            }
        }

        stage('Docker Compose Build') {
            steps {
                script {
                    def composeDir = WORK_DIR
                    echo "Docker Compose in ${composeDir}"

                    dir(composeDir) {
                        sh '''
                            echo "=== Docker Compose Build ==="
                            ls -la
                            # docker-compose build
                            echo "Simulate docker-compose build..."
                        '''
                    }
                }
            }
        }

        stage('Docker Compose Up') {
            steps {
                script {
                    def composeDir = WORK_DIR
                    dir(composeDir) {
                        sh '''
                            echo "=== Docker Compose Up ==="
                            # docker-compose up -d
                            echo "Simulate docker-compose up..."
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
    }
}
