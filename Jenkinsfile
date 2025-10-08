pipeline {
    agent any

    environment {
        // Jenkins Credential storing SonarQube Admin Token
        SONAR_TOKEN = credentials('SONAR_TOKEN')
        SONAR_HOST_URL = 'http://3.80.136.249:9000'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/Anvesh-ansh259/sonarqube-project.git', 
                    credentialsId: 'github-pat'
            }
        }

        stage('Compile Java') {
            steps {
                sh 'mkdir -p target/classes'
                sh 'javac -d target/classes src/main/java/*.java'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { 
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=JavaApp \
                        -Dsonar.projectName=JavaApp \
                        -Dsonar.sources=src/main/java \
                        -Dsonar.java.binaries=target/classes \
                        -Dsonar.host.url=${SONAR_HOST_URL} \
                        -Dsonar.token=${SONAR_TOKEN}
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed! Check SonarQube report."
        }
    }
}
