pipeline {
    agent any

    tools {
        nodejs 'NodeJS 18'  // le nom configuré dans Jenkins Tools
    }

    stages {
        stage('Install Dependencies') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }
    }
}
