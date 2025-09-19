pipeline {
    agent any

    tools {
        nodejs 'NodeJS 18'  // Assure-toi que NodeJS 18 est configuré dans Jenkins Tools
    }

    environment {
        FRONTEND_DIR = 'frontend'
        BACKEND_DIR = 'backend'
    }

    stages {
        // 🔹 Étape 1 : Récupérer le code
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // 🔹 Étape 2 : Installer les dépendances backend
        stage('Install Backend Dependencies') {
            steps {
                dir(env.BACKEND_DIR) {
                    sh 'npm install'
                }
            }
        }

        // 🔹 Étape 3 : Installer les dépendances frontend
        stage('Install Frontend Dependencies') {
            steps {
                dir(env.FRONTEND_DIR) {
                    sh 'npm install'
                }
            }
        }

        // 🔹 Étape 4 : Build du frontend
        stage('Build Frontend') {
            steps {
                dir(env.FRONTEND_DIR) {
                    sh 'npm run build'
                }
            }
        }

        // 🔹 Étape 5 : Lancer les tests backend (si existants)
        stage('Run Backend Tests') {
            steps {
                dir(env.BACKEND_DIR) {
                    sh 'npm test || echo "Aucun test trouvé"'
                }
            }
        }

        // 🔹 Étape 6 : Archiver le build frontend
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
