pipeline {
    agent { label 'sonarqube-node' } // your agent

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Anvesh-ansh259/sonarqube-project.git', credentialsId: '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'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // This will use the SonarQube server configuration you added in Jenkins
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner'
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
            echo "SonarQube scan completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check SonarQube report."
        }
    }
}
