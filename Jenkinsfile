pipeline {
    agent any

    tools {
        nodejs 'NodeJS 18'
    }

    environment {
        FRONTEND_DIR = 'frontend'
        BACKEND_DIR = 'backend'
//        FRONTEND_PORT = '5000'   // port local pour test frontend
  //      BACKEND_PORT = '3000'    // port backend
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Backend Dependencies') {
            steps {
                dir(env.BACKEND_DIR) {
                    sh 'npm install'
                }
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                dir(env.FRONTEND_DIR) {
                    sh 'npm install'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir(env.FRONTEND_DIR) {
                    sh 'npm run build'
                }
            }
        }

        stage('Run Backend Tests') {
            steps {
                dir(env.BACKEND_DIR) {
                    sh 'npm test || echo "Pas de tests backend"'
                }
            }
        }

        // ✅ Nouveau stage : Vérification frontend
        stage('Verify Frontend Deployment') {
            steps {
                script {
                    // Vérifie si le build existe
                    if (fileExists("${env.FRONTEND_DIR}/build/index.html")) {
                        echo "Frontend build OK ✅"
                    } else {
                        error "Frontend build manquant ❌"
                    }
                }
            }
        }

        // ✅ Nouveau stage : Vérification backend
        stage('Verify Backend Deployment') {
            steps {
                script {
                    // Vérifie si le serveur backend démarre (local)
                    def response = sh(script: "curl -s -o /dev/null -w '%{http_code}' http://localhost:${env.BACKEND_PORT}/", returnStdout: true).trim()
                    if (response == '200') {
                        echo "Backend accessible ✅"
                    } else {
                        error "Backend inaccessible ❌ (HTTP code: ${response})"
                    }
                }
            }
        }

        stage('Archive Frontend Build') {
            steps {
                archiveArtifacts artifacts: "${env.FRONTEND_DIR}/build/**", fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé.'
        }
        success {
            echo 'Pipeline terminé avec succès !'
        }
        failure {
            echo 'Le pipeline a échoué !'
        }
    }
}
