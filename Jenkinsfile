pipeline {
    agent any

    environment {
        FRONTEND_PORT = '3000'
        BACKEND_PORT  = '5000'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Récupération du code depuis Git'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installation frontend et backend'
                dir('frontend') {
                    sh 'npm install'
                }
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                echo 'Build du frontend'
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }

        stage('Start Backend & Frontend') {
            steps {
                script {
                    echo 'Lancement du backend en arrière-plan'
                    dir('backend') {
                        sh 'nohup npm start &'
                    }

                    echo 'Lancement du frontend en arrière-plan'
                    dir('frontend') {
                        sh 'nohup npm start &'
                    }

                    echo 'Attente que les services démarrent...'
                    sh 'sleep 10'

                    echo 'Vérification backend'
                    sh "curl -I http://localhost:${BACKEND_PORT}"

                    echo 'Vérification frontend'
                    sh "curl -I http://localhost:${FRONTEND_PORT}"
                }
            }
        }

        stage('Backend Tests') {
            steps {
                echo 'Exécution des tests backend'
                dir('backend') {
                    sh 'npm test'
                }
            }
        }

        stage('Optional Headless Frontend Test') {
            steps {
                echo 'Test rapide du frontend avec Puppeteer (headless)'
                dir('frontend') {
                    sh 'npm run test:ui || echo "Test UI headless terminé"'
                }
            }
        }

        stage('Deploy (optional)') {
            steps {
                echo 'Déploiement (Docker ou autre)'
                // sh 'docker-compose up -d'  # si tu utilises Docker
            }
        }
    }

    post {
        always {
            echo 'Arrêt des serveurs pour libérer les ports'
            sh "pkill -f 'npm start' || true"
        }
        success {
            echo 'Pipeline terminé avec succès !'
        }
        failure {
            echo 'Pipeline échoué.'
        }
    }
}
