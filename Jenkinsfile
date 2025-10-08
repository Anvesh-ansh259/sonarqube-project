pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'SonarScanner' // The name of SonarScanner in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/<your-username>/simple-sonar-project.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // Name of SonarQube server in Jenkins
                    sh "${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=simple-sonar-project \
                        -Dsonar.sources=src \
                        -Dsonar.host.url=http://<sonarqube-server-ip>:9000 \
                        -Dsonar.login=<your-sonarqube-token>"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
