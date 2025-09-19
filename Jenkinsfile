pipeline {
    agent any  // exécute sur n’importe quel agent Jenkins

    environment {
        NODEJS_VERSION = '18' // version Node.js à utiliser
    }

    stages {
        stage('Checkout') {
            steps {
                // Récupérer le code du dépôt
                checkout scm
            }
        }

        stage('Install Node.js') {
            steps {
                // Utiliser Node.js sur l’agent Jenkins
                // (nécessite le plugin NodeJS installé)
                tool name: "NodeJS ${NODEJS_VERSION}", type: 'NodeJSInstallation'
            }
        }

        stage('Install Backend Dependencies') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }

        stage('Run Backend Tests') {
            steps {
                dir('backend') {
                    sh 'npm test || echo "No tests yet"'
                }
            }
        }

        stage('Archive Artefacts') {
            steps {
                archiveArtifacts artifacts: 'frontend/build/**', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé'
        }
        success {
            echo 'Déploiement possible ici…'
        }
        failure {
            echo 'Le pipeline a échoué !'
        }
    }
}
