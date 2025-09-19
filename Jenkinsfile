pipeline {
    agent any

    environment {
        NODE_VERSION = '18'
        FRONTEND_PORT = '3000'  // Port utilisé par React
        BACKEND_PORT  = '5000'  // Port utilisé par le backend
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install & Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Install Backend') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Start Backend & Frontend for Testing') {
            steps {
                script {
                    // Démarrer backend en arrière-plan
                    dir('backend') {
                        sh 'nohup npm start &'
                    }

                    // Démarrer frontend en arrière-plan (si tu veux tester avec dev server)
                    dir('frontend') {
                        sh 'nohup npm start &'
                    }

                    // Attendre quelques secondes pour que les services soient prêts
                    sh 'sleep 10'

                    // Vérifier que les ports répondent
                    sh "curl -I http://localhost:${BACKEND_PORT}"
                    sh "curl -I http://localhost:${FRONTEND_PORT}"
                }
            }
        }

        stage('Tests Unitaires') {
            steps {
                dir('backend') {
                    sh 'npm test'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Déploiement du projet (Docker ou autre)'
            }
        }
    }

    post {
        always {
            echo 'Arrêt des services pour libérer les ports'
            sh "pkill -f 'npm start'"  // Arrête tous les serveurs npm
        }
    }
}
