pipeline {
    agent { label 'sonarqube-node' } // your agent label

    environment {
        SONARQUBE = 'SonarQube'        // Name of SonarQube installation in Jenkins
        SONAR_TOKEN = credentials('sonartoken') // Jenkins credential ID for SonarQube token
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repo using GitHub PAT
                git url: 'https://github.com/Anvesh-ansh259/sonarqube-project.git',
                    branch: 'main',
                    credentialsId: 'github-pat'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(SONARQUBE) {
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=JavaApp \
                        -Dsonar.projectName=JavaApp \
                        -Dsonar.sources=src/main/java \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://3.80.136.249:9000 \
                        -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Wait for SonarQube Quality Gate result
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline finished successfully! ✅"
        }
        failure {
            echo "Pipeline failed! ❌ Check SonarQube report."
        }
    }
}
