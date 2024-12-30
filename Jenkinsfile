pipeline {
    agent any
    tools {
         nodejs 'nodejs'
    }
    environment {
        SONARQUBE_SERVER = 'sonarqube-server' // Name of your SonarQube server in Jenkins global tool configuration
        SONARQUBE_SCANNER = 'sonarscanner'  // Name of your Sonar Scanner installation in Jenkins global tool configuration
        PATH = "${PATH};C:\\Windows\\System32"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                bat '''
                npm install
                npm init -y
                git add package.json
                git commit -m "Add package.json"
                git push origin main
                '''
            }
        }

        stage('SonarAnalysis') {
            environment {
                SONAR_TOKEN = credentials('sonarqube-credential')
            }
            steps {
                bat '''
                sonar-scanner -Dsonar.projectKey=frontend ^
                -Dsonar.sources=. ^
                -Dsonar.host.url=http://localhost:9000 ^
                -Dsonar.token=%SONAR_TOKEN%
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
