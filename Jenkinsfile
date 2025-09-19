pipeline {
    agent any

    environment {
        // Définit un chemin par défaut si aucune valeur n'est fournie
        MY_WORKSPACE = "${env.WORKSPACE ?: '.'}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Exemple d'utilisation sécurisée de pushd
                    def myDir = MY_WORKSPACE
                    if (!myDir) {
                        error "Le chemin de travail est nul !"
                    }

                    // pushd sécurisé : si myDir est null, utilise '.'
                    pushd(myDir ?: '.') {
                        sh 'ls -la'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                dir(MY_WORKSPACE ?: '.') {
                    sh 'echo "Tests en cours..."'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def deployDir = "${env.DEPLOY_PATH ?: '.'}"
                    dir(deployDir) {
                        sh 'echo "Déploiement dans ${deployDir}"'
                    }
                }
            }
        }
    }
}
