pipeline {
    agent { label 'sonarqube-node' } // your agent name

    // Use tools configured in Jenkins Global Tool Configuration
    tools {
        maven 'Maven3'   // exact name from Jenkins Maven config
        jdk 'Java17'     // exact name from Jenkins JDK config
    }

    environment {
        // SonarQube server configured in Jenkins Global Tool Configuration
        SONARQUBE_ENV = 'SonarQube' 
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Anvesh-ansh259/sonarqube-project.git',
                    credentialsId: 'github-pat' // your GitHub credentials ID in Jenkins
            }
        }

        stage('Build') {
            steps {
                echo 'Running Maven Build...'
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube Analysis...'
                // Use SonarQube environment configured in Jenkins
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                echo 'Waiting for SonarQube Quality Gate...'
                timeout(time: 1, unit: 'HOURS') { // timeout if SQ server takes long
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
